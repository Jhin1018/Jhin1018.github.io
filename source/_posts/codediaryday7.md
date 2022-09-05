---
title: 刷题日记 day7 
date: 2021-05-09 01:03:17
tags: LeetCode
categories: 技术
description: LeetCode 1723 and 25
---

## LeetCode  1723 Find Minimum Time to Finish All Jobs

今天的每日一题，体感是hard中也比较难的题目，放个官方题解链接以后再消化吧。[官方题解](https://leetcode-cn.com/problems/find-minimum-time-to-finish-all-jobs/solution/wan-cheng-suo-you-gong-zuo-de-zui-duan-s-hrhu/)

## LeetCode 25 Reverse Nodes in k-Group

虽然也是hard题，但是面试常考，所以研究一下。

### 思路

翻转一个链表并不难，可以看[day3](http://eccentric.icu/2021/05/04/codediaryday3/)解反转链表的思路。这道题最复杂的是长度为k的子链表翻转后如何再接回原链表中。首先先来写这个翻转子链表的函数，这个函数和普通的翻转链表没啥不同，只需要考虑几个细节即可。接下来我们就要考虑翻转后的子链表如何接回原链表中。在每次翻转k个节点之前，我们需要一个pre节点来保存子链表的前一个节点，和一个next 节点保存子链表的后一个节点，这样就可以接回去了。那么问题又来了，第一个节点前面没有怎么办？我们就人为的给第一个节点前面加上hair，这样就可以保持整个循环结构中的一致性，而且还有另一个好处，那就是hair.next一直指向这个翻转过的链表的头结点。至此，所有的问题都解决了。

### Code

```java
public ListNode reverseKGroup(ListNode head, int k) {
    ListNode hair= new ListNode(0);
    hair.next = head;
    ListNode pre =hair;//子链表的前一个节点

    while(head !=null){
        ListNode tail = pre;
        for(int i=0;i<k;i++){//从上一次的尾巴往前走k步，和从head走k-1步一样
            tail = tail.next;
            if(tail == null){
                return hair.next;//最后不足k个不需要翻转
            }
        }
        ListNode next = tail.next;//子链表的后一个节点

        ListNode[] subchain = myreverse(head,tail);//子链表翻转

        head = subchain[0];//翻转后的头
        tail = subchain[1];//翻转后的尾
        pre.next = head;//子链表的前一个节点接上新的头
        tail.next = next;//尾巴接上原本的后一个节点，其实在myreverse函数中已经接好了，这一行可有可无的
        pre = tail;//对于下一个循环中的子链表，这一次的尾巴就是下一次的pre
        head = next;
    }
    return hair.next;
}

public ListNode[] myreverse(ListNode head,ListNode tail){
    ListNode prev = tail.next;//翻转链表中这里是null，而初始的prev表示head在翻转后应指向的节点，所以是tail的next
    ListNode cur = head;
    while(prev != tail){//留意这个循环终止条件，和反转链表不同
        ListNode next = cur.next;
        cur.next = prev;
        prev = cur;
        cur = next;
    }
    return new ListNode[]{tail,head};// 翻转后head和tail互换
}
```

- 时间复杂度:O(n)
- 空间复杂度:O(1)

