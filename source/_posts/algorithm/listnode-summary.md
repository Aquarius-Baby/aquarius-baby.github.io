---
title: 链表题解 date: 2020-02-10 15:44:53 categories:

- 算法 tags:
- 链表

---

链表题解整理：

- 找到中间节点
- 删除倒数第k个链表
- 合并2个链表
- 反转链表
- 反转链表前K个节点
- 反转链表部分节点
- K个一组反转链表

<!--more-->

```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null) return;
        // 1.找到中间节点
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode reverse = slow.next;
        slow.next = null;

        // 2.反转后半部分链表
        ListNode pre = null;
        while (reverse != null) {
            ListNode next = reverse.next;
            reverse.next = pre;
            pre = reverse;
            reverse = next;
        }

        // 3.合并2个链表
        ListNode cur = head;
        ListNode start1 = head.next;
        ListNode start2 = pre;
        while (start1 != null && start2 != null) {
            cur.next = start2;
            cur = cur.next;
            start2 = start2.next;

            cur.next = start1;
            cur = cur.next;
            start1 = start1.next;
        }
        if (start2 != null) {
            cur.next = start2;
        }
    }
}
```

# 找到中间节点

1 -> 2 -> 3 -> 4 -> 5 返回3

1 -> 2 -> 3 -> 4 -> 5 -> 6 返回4

```java
class Solution {
    public void fingMiddleNode(ListNode head) {
        if (head == null) return;
        // 1.找到中间节点
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode mid = slow.next;
        return mid;
    }
}
```

# 删除倒数第k个节点 leetcode19

1 -> 2 -> 3 -> 4 -> 5 -> 6

当 q 指向 6，p指向4，q的 next == null , p的next就是需要被删除的

```java
class Solution {
    public ListNode removeKNode(ListNode head, int k) {
        if (head == null) return;
        ListNode p = head;
        ListNode q = head;
        // q前进 k 步
        while (k > 0) {
            q = q.next;
            k--;
        }
        // k步走完， q == null, 说明需要删除头节点
        if (q == null) {
            head = head.next;
            return head;
        }
        while (q.next != null) {
            p = p.next;
            q = q.next;
        }
        p.next = p.next.next;
        return head;
    }
}
```

# 合并2个有序链表 leetcode 21

1 -> 2 —> 5 -> 9 3 -> 4 —> 7 -> 9

得到 1 -> 2 —> 3 -> 4 -> 5 -> 7 -> 9 -> 9

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (null == l1) {
            return l2;
        }
        if (null == l2) {
            return l1;
        }
        ListNode res = new ListNode(0);
        ListNode newHead = res;
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                newHead.next = l1;
                l1 = l1.next;
            } else {
                newHead.next = l2;
                l2 = l2.next;
            }
            newHead = newHead.next;
        }
        if (l1 != null) {
            newHead.next = l1;
        }
        if (l2 != null) {
            newHead.next = l2;
        }
        return res.next;
    }
}
```

# 反转链表 leetcode 206

1 -> 2 -> 3 -> 4 -> 5 得到 5 -> 4 -> 3 -> 2 -> 1

```java
class Solution {
    public ListNode reverseNode(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode pre = null;
        ListNode cur = head;
        ListNode next;
        while (cur != null) {
            next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}

```

# 反转链表前K个节点

1 -> 2 -> 3 -> 4 -> 5 , k = 3

得到：3 -> 2 -> 1 -> 4 -> 5

```java
class Solution {
    public ListNode reverseNode(ListNode head, int k) {
        if (head == null) {
            return head;
        }
        ListNode dummy = new ListNode(0);
        ListNode pre = dummy;
        dummy.next = head;
        ListNode next;
        while (k > 0) {
            next = head.next;
            head.next = pre;
            pre = head;
            head = next;
            k--;
        }
        dummy.next.next = next;
        dummy.next = pre;
        return dummy.next;
    }
}
```

# 反转链表部分节点 leecode 92

反转left ，right 之间的节点

1 -> 2 -> 3 -> 4 -> 5 -> 6, left = 2, right = 4

得到：1 -> 4 -> 3 -> 2 -> 5 -> 6

```java

class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode dummy = new ListNode(0);
        ListNode pre = dummy;
        dummy.next = head;
        int i = 0;
        while (i < left - 1) {
            i++;
            pre = pre.next;
        }
        // pre.next 是需要处理的节点
        ListNode cur = pre.next;
        ListNode next = null;
        ListNode temp = null;

        while (i < right) {
            next = cur.next;
            cur.next = temp;
            temp = cur;
            cur = next;
            i++;
        }
        pre.next.next = next;
        pre.next = temp;
        return dummy.next;
    }
}
```

# K个一组反转链表 leecode 25

1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 , k = 3

得到：3 -> 2 -> 1 -> 6 -> 5 -> 4 -> 7

```java

class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode pre = dummy;
        ListNode end = dummy;
        while (end.next != null) {
            for (int i = 0; i < k && end != null; i++)
                end = end.next;
            if (end == null)
                break;
            ListNode start = pre.next;
            ListNode next = end.next;
            end.next = null;
            pre.next = reverse(start, k);
            start.next = next;
            pre = start;
            end = pre;
        }
        return dummy.next;
    }

    private ListNode reverse(ListNode start, int k) {
        ListNode pre = null;
        ListNode curr = start;
        int i = 0;
        while (i < k) {
            ListNode next = curr.next;
            curr.next = pre;
            pre = curr;
            curr = next;
            i++;
        }
        return pre;
    }
}
```