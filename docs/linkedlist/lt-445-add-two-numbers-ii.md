# 445.两数相加II
题目链接：[传送门](https://leetcode-cn.com/problems/add-two-numbers-ii/)

## 题目描述：
给你两个**非空**链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。

你可以假设除了数字`0`之外，这两个数字都不会以零开头。

**进阶**：如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：先使用两个栈分别将各自链表中的节点值压入栈中，然后同时一起出栈，并用**头插法**将创建新的节点插入单链表头，直到2个栈为空为止，最后特判一下是否还有进位即可！

## AC代码：
```java
class Solution {
	public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
		Stack<Integer> stack1 = new Stack<Integer>();
		Stack<Integer> stack2 = new Stack<Integer>();
		ListNode cur1 = l1, cur2 = l2;
		while (cur1 != null) {
			stack1.add(cur1.val);
			cur1 = cur1.next;
		}
		while (cur2 != null) {
			stack2.add(cur2.val);
			cur2 = cur2.next;
		}
		ListNode head = null, cur;
		int carry = 0, sum;
		while (!stack1.isEmpty() || !stack2.isEmpty()) {
			sum = (stack1.isEmpty() ? 0 : stack1.pop()) + (stack2.isEmpty() ? 0 : stack2.pop()) + carry;
			carry = sum / 10;
			cur = new ListNode(sum % 10);
			cur.next = head;
			head = cur;
		}
		if (carry > 0) { // 剩下一位进位
			cur = new ListNode(carry);
			cur.next = head;
			head = cur;
		}
		return head;
	}
}
```