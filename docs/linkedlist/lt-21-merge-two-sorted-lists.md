# 21.合并两个有序链表
题目链接：[传送门](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

## 题目描述：
将两个升序链表合并为一个新的**升序**链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例**：

- 输入：`1->2->4`, `1->3->4`
- 输出：`1->1->2->3->4->4`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：简单单链表操作。

## AC代码：
```java
class Solution {
	public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
		ListNode head = new ListNode(0), cur = head;
		while (l1 != null && l2 != null) {
			cur.next = new ListNode(0);
			if (l1.val < l2.val) {
				cur.next.val = l1.val;
				l1 = l1.next;
			} else {
				cur.next.val = l2.val;
				l2 = l2.next;
			}
			cur = cur.next;
		}
		while (l1 != null) {
			cur.next = new ListNode(l1.val);
			l1 = l1.next;
			cur = cur.next;
		}
		while (l2 != null) {
			cur.next = new ListNode(l2.val);
			l2 = l2.next;
			cur = cur.next;
		}
		return head.next;
	}
}
```