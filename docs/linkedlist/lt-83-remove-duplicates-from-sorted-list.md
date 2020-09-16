# 83.删除排序链表中的重复元素
题目链接：[传送门](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

## 题目描述：
给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：单链表简单遍历，每次遍历时检查下一个节点是否有相同值，若有则将当前节点指向下下个节点，否则移动到下一个节点。

## AC代码：
```java
class Solution {
	public ListNode deleteDuplicates(ListNode head) {
		ListNode cur = head;
		while (cur != null && cur.next != null) {
			if (cur.val == cur.next.val) {
				cur.next = cur.next.next;
			} else
				cur = cur.next;
		}
		return head;
	}
}
```