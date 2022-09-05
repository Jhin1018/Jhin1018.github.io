---
title: 刷题日记day3
date: 2021-05-04 22:49:18
tags: LeetCode
categories: 技术
---

## LeetCode 每日一题 1473 粉刷房子 III 

这道hard的题目，要用到三维dp，对于我现在来说还是太难了，这里放个题解连接后面慢慢消化。

[题解](https://leetcode-cn.com/problems/paint-house-iii/solution/gong-shui-san-xie-san-wei-dong-tai-gui-h-ud7m/)

## LeetCode 206 反转链表

面试常考题，上次网易面试里也被考到了，我给了一个用栈来实现的解答，面试官的反馈来看感觉空间复杂度不够满意，今天来学习一下优秀的思路。

主要的思路就是用三个指针，分别指向当前节点、前一个节点、后一个节点。然后把当前节点的next改为pre，然后向前移动当前节点的指针并更新另外两个指针相应位置。

```
public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode next = curr.next;\\保存当前节点的后一个节点，不然没法向前移动
            curr.next = pre;//翻转
            pre = curr;//向前移动当前节点指针前先移动前一个节点的指针，让它指向当前节点
            curr = next;
        }
        return pre;
    }
```

- 时间复杂度：O(n)，其中 n 是链表的长度。需要遍历链表一次。

- 空间复杂度：O(1)。

  