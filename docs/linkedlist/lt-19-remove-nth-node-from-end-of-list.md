# 19.删除链表的倒数第N个节点
题目链接：[传送门](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

## 题目描述：
给定一个链表，删除链表的倒数第`n`个节点，并且返回链表的头结点。

**说明**：给定的`n`保证是有效的。

**进阶**：你能尝试使用一趟扫描实现吗？

**示例**：

- 给定一个链表：`1->2->3->4->5`, 和`n = 2`。
- 当删除了倒数第二个节点后，链表变为`1->2->3->5`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：双指针。指针`cur1`先定位第$n+1$个节点的位置，再和指针`cur2`一起遍历，直到`cur1==null`，此时`cur1`正好指向待删除节点的前一个节点的位置。

## AC代码：
```java
class Solution {
	public ListNode removeNthFromEnd(ListNode head, int n) {
		ListNode res = new ListNode(0), cur1 = res, cur2 = res;
		res.next = head;
		// 从值为0的节点开始遍历，避免后面遇到空指针
		for (int i = 1; i <= n + 1; i++) {
			cur1 = cur1.next;
		}
		// 若n为链表长度，则此时的cur1指针为空
		while (cur1 != null) {
			cur1 = cur1.next;
			cur2 = cur2.next;
		}
        // 删除目标节点
		cur2.next = cur2.next.next;
		return res.next;
	}
}
```