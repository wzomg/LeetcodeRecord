# 92.反转链表II
题目链接：[传送门](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

## 题目描述：
反转从位置`m`到`n`的链表。请使用一趟扫描完成反转。

**说明**：$1 \leq m \leq n \leq$ 链表长度。

**示例**：

- 输入: `1->2->3->4->5->NULL`, `m = 2`, `n = 4`
- 输出: `1->4->3->2->5->NULL`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：简单链表操作，虚拟2个头节点，将要反转的部分采用**头插法**即可。

## AC代码：
```java
class Solution {
	public ListNode reverseBetween(ListNode head, int m, int n) {
		// 一进来就判断是否为空等不满足的一些条件
		if (head == null || m > n)
			return null;
		// 虚拟2个头节点，便于操作
		ListNode cur = head, resHead = new ListNode(-1), pre1 = resHead;
		// dummyHead 时刻指向第 n 至 m 个节点反转后的虚拟头节点
		ListNode dummyHead = new ListNode(0), pre2 = dummyHead, tmp1, tmp2;
		int pos = 0;
		while (cur != null) {
			pos++;
			if (pos < m) {
				pre1.next = cur;
				pre1 = pre1.next;
				cur = cur.next;
			} else if (pos <= n) {
				// 老老实实先取出下一个节点
				tmp1 = cur.next;
				// 使用头插法
				// 先取出虚拟头节点的下一个节点
				tmp2 = pre2.next;
				// 将当前节点指向虚拟头节点指向的下一个节点
				cur.next = tmp2;
				// 将虚拟头节点重新指向新的节点
				pre2.next = cur;
				// cur 归位
				cur = tmp1;
			} else
				break;
		}
		// 遍历到 [m,n] 反转后的尾节点
		while (pre2.next != null)
			pre2 = pre2.next;
		// 将尾节点指向第m个节点后剩余节点链表的头节点
		pre2.next = cur;
		// 将第 n 个节点的上一个节点指向 第2个虚拟头节点的下一个节点
		pre1.next = dummyHead.next;
		return resHead.next;
	}
}
```