# 328.奇偶链表
题目链接：[传送门](https://leetcode-cn.com/problems/odd-even-linked-list/)

## 题目描述：
给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为`O(1)`，时间复杂度应为`O(nodes)`，`nodes`为节点总数。

**说明**:

- 应当保持奇数节点和偶数节点的相对顺序。
- 链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：简单链表操作，用4个指针，2个头指针，2个移动指针。注意判断原链表节点的个数是奇数还是偶数个！

## AC代码：
```java
class Solution {
	public ListNode oddEvenList(ListNode head) {
		if (head == null) // 每次要先判空
			return null;
		// 注意：pre 初始要指向头节点，以防只有原链表只有2个节点时，若 pre 为空，则第1个节点不能连接上第2个节点
		ListNode fist = head, cur1 = fist, second = head == null ? null : head.next, cur2 = second, pre = head, tmp;
		// 两两指针走
		while (cur1 != null && cur2 != null) {
			// 老老实实先拿出第3个节点后剩余的链表头节点
			tmp = cur2.next;
			// 将第1个节点指向第3个节点
			cur1.next = tmp;
			// 若第3个节点不为空，则将第2个节点指向第4个节点
			if (tmp != null)
				cur2.next = tmp.next;
			// 记录奇数节点链表的最后一个节点
			if (tmp != null)
				pre = tmp;
			// 归位指针：第1个指针移动到第3个节点
			cur1 = tmp;
			// 第2个指针移到第四个节点
			cur2 = tmp == null ? null : tmp.next;
		}
		// 若 cur1 不为空，说明原链表节点个数为奇数，将其尾节点直接指向偶数节点链表
		if (cur1 != null)
			cur1.next = second;
		else // 否则说明原链表节点的个数为偶数，将标记奇数节点链表的最后一个节点指向偶数节点链表的头节点
			pre.next = second;
		return head;
	}
}
```
