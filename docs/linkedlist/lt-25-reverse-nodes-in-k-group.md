# 25.K个一组翻转链表
题目链接：[传送门](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

## 题目描述：
给你一个链表，每`k`个节点一组进行翻转，请你返回翻转后的链表。

`k`是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是`k`的整数倍，那么请将最后剩余的节点保持原有顺序。

**说明**：

- 你的算法只能使用常数的额外空间。
- **你不能只是单纯的改变节点内部的值**，而是需要实际进行节点交换。

**示例**：

- 给你这个链表：`1->2->3->4->5`
- 当 $k=2$ 时，应当返回: `2->1->4->3->5`
- 当 $k=3$ 时，应当返回: `3->2->1->4->5`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：先确定要遍历`k`的整数倍个节点数量：`limit`。虚拟一个头节点：`dummyCur`，若`i % k != 0`，则使用头插法插入到`dummyCur`与`dummyCur`的下一个节点之间，否则就将`dummyCur`移动到尾节点，直到遍历完`limit`个节点，最后将`dummyCur.next`指向剩余链表节点的头节点即可！

## AC代码：
```java
class Solution {
	public ListNode reverseKGroup(ListNode head, int k) {
		if (head == null) // 无论怎样，先判断是否满足条件
			return null;
		ListNode dummyHead = new ListNode(-1), dummyCur = dummyHead;
		int cnt = 0, tot = 0, limit = 0;
		ListNode cur = head, nex, tmp;
		while (cur != null) {
			tot++;
			cur = cur.next;
		}
		limit = tot / k * k;
		cur = head;
		while (cnt < limit) { // 只遍历k的整数倍
			cnt++;
			// 先保存下一个即将遍历的节点
			tmp = cur.next;
			// 采用头插法插入一个节点
			nex = dummyCur.next;
			cur.next = nex;
			// 将虚拟头节点的下一个节点指向当前遍历的节点 cur
			dummyCur.next = cur;
			// 若刚好为k的整数倍，则移动虚拟头节点
			if (cnt % k == 0) {
				while (dummyCur.next != null)
					dummyCur = dummyCur.next;
			}
			// 归位指针，继续遍历
			cur = tmp;
		}
		dummyCur.next = cur;
		return dummyHead.next;
	}
}
```
