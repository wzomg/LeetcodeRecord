# 面试题02.06.回文链表
题目链接：[传送门](https://leetcode-cn.com/problems/palindrome-linked-list-lcci/)

## 题目描述：
编写一个函数，检查输入的链表是否是回文的。

**进阶**：你能否用 $O(n)$ 时间复杂度和 $O(1)$ 空间复杂度解决此题？

**示例**：

- 输入：`1->2`
- 输出：`false` 

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：快慢指针实现。反转后半部分的链表，再和前半部分从头开始依次比较节点的值即可！

## AC代码：
```java
class Solution {
	public boolean isPalindrome(ListNode head) {
		if (head == null)
			return true;
		ListNode slow = head, fast = head.next;
		// 找到后半部分链表的前一个节点 slow，然后将后半部分链表翻转
		while (fast != null && fast.next != null) {
			slow = slow.next;
			fast = fast.next.next;
		}
		ListNode pre = null, cur = slow.next, tmp;
		while (cur != null) {
			tmp = cur.next;
			cur.next = pre;
			pre = cur;
			cur = tmp;
		}
		ListNode cur1 = head, cur2 = pre;
		while (cur2 != null) {
			if (cur1.val != cur2.val)
				return false;
			cur1 = cur1.next;
			cur2 = cur2.next;
		}
		return true;
	}
}
```