---
title: LRU/LFU
categories:
  - 笔记
tags:
  - 算法
  - 面试
date: 2021-01-26 20:22:17
---
# LRU
Least Recently Used，最近最久未使用算法

是一种常见的缓存算法，在很多分布式缓存系统（如Redis, Memcached）中都有广泛使用。

LRU算法的思想是：如果一个数据在最近一段时间没有被访问到，那么可以认为在将来它被访问的可能性也很小。因此，当空间满时，最久没有访问的数据最先被置换（淘汰）。

LRU算法的描述： 设计一种缓存结构，该结构在构造时确定大小，假设大小为 K，并有两个功能：

set(key,value)：将记录(key,value)插入该结构。当缓存满时，将最久未使用的数据置换掉。
get(key)：返回key对应的value值。
实现：
最朴素的思想就是用数组+时间戳的方式，不过这样做效率较低。
因此，我们可以用双向链表（LinkedList）+哈希表（HashMap）实现（链表用来表示位置，哈希表用来存储和查找），在Java里有对应的数据结构LinkedHashMap。

```java
class LRU<K, V> extends LinkedHashMap<K, V> {
    private int cacheSize;
    public LRU(int cacheSize) {
        super(16, (float) 0.75, true);
        this.cacheSize = cacheSize;
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > cacheSize;
    }

    public static void main(String[] args) {
        LRU<String, String> lru = new LRU<String, String>(3);
        lru.put("1","a");
        lru.put("2","b");
        lru.put("3","c");

        System.out.println(lru.toString());

        lru.put("4","d");
        System.out.println(lru.toString());
        lru.put("1","d");
        System.out.println(lru.toString());
    }
}
```

# LFU
Least Frequently Used，最近最少使用算法
找到队列中使用频率最少

```java
public class LFU<K, V> {
    private final int capcity;

    private Map<K, V> cache = new HashMap<>();

    private Map<K, HitRate> count = new HashMap<>();

    public LFU(int capcity) {
        this.capcity = capcity;
    }

    public void put(K key, V value) {
        V v = cache.get(key);
        if (v == null) {
            if (cache.size() == capcity) {
                removeElement();
            }
            count.put(key, new HitRate(key, 1, System.nanoTime()));
        } else {
            addHitCount(key);
        }
        cache.put(key, value);
    }

    public V get(K key) {
        V value = cache.get(key);
        if (value != null) {
            addHitCount(key);
            return value;
        }
        return null;
    }

    //移除元素
    private void removeElement() {
        HitRate hr = Collections.min(count.values());
        cache.remove(hr.key);
        count.remove(hr.key);
    }

    //更新访问元素状态
    private void addHitCount(K key) {
        HitRate hitRate = count.get(key);
        hitRate.hitCount = hitRate.hitCount + 1;
        hitRate.lastTime = System.nanoTime();
    }

    //内部类
    class HitRate implements Comparable<HitRate> {
        private K key;
        private int hitCount;
        private long lastTime;

        private HitRate(K key, int hitCount, long lastTime) {
            this.key = key;
            this.hitCount = hitCount;
            this.lastTime = lastTime;
        }

        @Override
        public int compareTo(HitRate o) {
            int compare = Integer.compare(this.hitCount, o.hitCount);
            return compare == 0 ? Long.compare(this.lastTime, o.lastTime) : compare;
        }
    }

    public static void main(String[] args) {
        LFU<Integer, Integer> cache = new LFU<>(3);
        cache.put(2, 2);
        cache.put(1, 1);
        System.out.println(cache.get(2));
        System.out.println(cache.get(1));
        System.out.println(cache.get(2));
        cache.put(3, 3);
        cache.put(4, 4);
        //1、2元素都有访问次数，放入3后缓存满，加入4时淘汰3
        System.out.println(cache.get(3));
        System.out.println(cache.get(2));
        //System.out.println(cache.get(1));
        System.out.println(cache.get(4));

        cache.put(5, 5);
        //目前2访问2次，1访问一次，4访问一次，由于4的时间比较新，放入5的时候移除1元素。
        System.out.println("-=-=-=-");
        cache.cache.entrySet().forEach(entry -> {
            System.out.println(entry.getValue());
        });
    }
}
```

