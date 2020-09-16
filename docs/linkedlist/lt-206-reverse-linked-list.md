# 206.反转链表
题目链接：[传送门](https://leetcode-cn.com/problems/reverse-linked-list/)

## 题目描述：
反转一个单链表。

**示例**:

- 输入: `1->2->3->4->5->NULL`
- 输出: `5->4->3->2->1->NULL`

**进阶**：你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：迭代法。用一个节点时时指向新链表的头节点，即跟在遍历指针的屁股位置，每次将cur.next指向新链表表头，注意指针的指向！递归法：每次返回反转后的链表表头，将反转后的链表尾节点指向当前节点，同时要将当前节点的next域置为空，否则最后会出现自环！

## AC代码1：迭代法
```java
class Solution {
	public ListNode reverseList(ListNode head) {
		ListNode cur = head, nexNode = null, newHead = null;
		while (cur != null) {
			// 先保存当前节点的下一个节点，此时cur.next已保存，避免断链
			nexNode = cur.next;
			// 将cur.next指向cur的前一个节点，此时newHead已经保存，需重新定位新链表表头节点为cur节点
			cur.next = newHead;
			// 重新定位新链表表头为cur
			newHead = cur;
			// cur重新指向链表剩下的头节点nexNode
			cur = nexNode;
		}
		return newHead;
	}
}
```

## AC代码2：递归法
```java
class Solution {
	public ListNode reverseList(ListNode head) {
		// 第一个判空是可能输入的链表头指针为空，第二个判空是递归终止条件
		if (head == null || head.next == null)
			return head;
		// 以1，2，3，4，5为例子，可以简单在大脑过一遍
		ListNode newLink = reverseList(head.next); // 接收新链表表头节点
        // 将当前节点的下一个节点（反转后的链表尾节点）指向当前节点
		head.next.next = head;
        // 然后设置当前节点的next域为空，避免到最后出现自环
		head.next = null;
		return newLink; // 每次弹出返回新链表的表头
	}
}
```