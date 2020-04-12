# 142.环形链表II
题目链接：[传送门](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

## 题目描述：
给定一个链表，返回链表开始入环的第一个节点。如果链表无环，则返回`null`。

为了表示给定链表中的环，我们使用整数`pos`来表示链表尾连接到链表中的位置（索引从`0`开始）。如果`pos`是`-1`，则在该链表中没有环。

**说明**：不允许修改给定的链表。

**进阶**：你是否可以不用额外空间解决此题？

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：快慢指针实现。假设环外的节点数为`x`，环的节点数是`C`，快慢指针第一次相遇距离环入口节点的距离为`y`，则慢指针走过的距离：$x+a \times C+y$。因为快指针的速度是慢指针的`2`倍，所以快指针走过的距离为 $2 \times (x+a \times C+y)$；另外，快慢指针走过的距离之差一定是`C`的整数倍，即 $x+a \times C+y = b \times C => x+y=(b-a) \times C$，说明从起点到第一次相遇节点的距离刚好为`C`的整数倍。因此，当两者第一次相遇时，用一个指针指向起点，将其与慢指针同步往向前走，直到两个指针相等即表示走到了环入口起点。

## AC代码：
```java
class Solution {
	public ListNode detectCycle(ListNode head) {
		if (head == null)
			return null;
		ListNode cur1 = head, cur2 = head, rcur = head;
		while (cur2 != null && cur2.next != null) {
			cur1 = cur1.next;
			cur2 = cur2.next.next;
			if (cur1 == cur2) {
				while (rcur != cur1) {
					rcur = rcur.next;
					cur1 = cur1.next;
				}
				return rcur;
			}
		}
		return null;
	}
}
```