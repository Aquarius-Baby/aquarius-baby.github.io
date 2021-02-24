---
title: 1-两数之和
categories:
    - 算法
tags:
    - leetcode
    - 数组
    - 哈希表
date: 2021-02-23 10:07:12
---

1 两数之和
2021-02-23 10:06:05

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

你可以按任意顺序返回答案。

示例 1：
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

示例 2：
输入：nums = [3,2,4], target = 6
输出：[1,2]


示例 3：
输入：nums = [3,3], target = 6
输出：[0,1]

提示：

2 <= nums.length <= 103
-109 <= nums[i] <= 109
-109 <= target <= 109
只会存在一个有效答案

Related Topics 数组 哈希表

### 解法1
双层for循环遍历数组，依次获取2个数a,b，判断 a+b 是否符合要求。
```java
class Solution {
        public int[] twoSum(int[] nums, int target) {
            for (int i = 0; i < nums.length - 1; i++) {
                for (int j = i + 1; j < nums.length; j++)
                    if (nums[i] + nums[j] == target) {
                        int[] res = new int[2];
                        res[0] = i;
                        res[1] = j;
                        return res;
                    }
            }
            return new int[2];
        }
    }
```
### 解法2
 a + b = target 
遍历数组，当前位置为 i，nums[i] = b, 已知 b 与 target, 如果可以找到另一个a和对应的index, 则可以找到答案。
使用hashMap来记录值与对应的index。


```java
class Solution {
        public int[] twoSum(int[] nums, int target) {
            Map<Integer, Integer> map = new HashMap<>();
            int[] res = new int[2];
            for (int i = 0; i < nums.length; i++) {
                if (map.containsKey(target - nums[i])) {
                    res[0] = map.get(target - nums[i]);
                    res[1] = i;
                    return res;
                } else {
                    map.put(nums[i], i);
                }
            }
            return res;
        }
    }
```