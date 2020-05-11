# 24.两两交换链表中的节点
题目链接：[传送门](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

## 题目描述：
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你**不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。

示例：

给定`1->2->3->4`, 你应该返回`2->1->4->3`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：简单链表操作，用2个指针一前一后同时移动，详解看代码注释！

## AC代码：
```java
class Solution {
	public ListNode swapPairs(ListNode head) {
		// 虚拟一个头节点，便于操作
		ListNode dummyHead = new ListNode(-1), dummyCur = dummyHead;
		// 两个指针同时移动
		ListNode first = head, second = first == null ? null : first.next, tmp;
		while (first != null && second != null) {
			// 先保存一下第3个链表节点
			tmp = second.next;
			// 将原第2个节点指向第1个节点
			second.next = first;
			// 将原第1个节点指向第3个节点
			first.next = tmp;
			// 将 first 上一个节点 dummyCur 指向 原第2个节点
			dummyCur.next = second;
			// 每次保存下一个 first 的上一个节点 dummyCur
			dummyCur = first;
			// 归位指针，指向下一次将要遍历的节点
			first = tmp;
			second = first == null ? null : first.next;
		}
		// 注意最后需要判断原来是否有奇数个节点
		if (first != null)
			dummyCur.next = first;
		return dummyHead.next;
	}
}
```
