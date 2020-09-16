# 160.相交链表
题目链接：[传送门](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

## 题目描述：
编写一个程序，找到两个单链表相交的起始节点。
如下面的两个链表：

![](../_media/160_statement.png)

在节点`c1`开始相交。

**注意**：

- 如果两个链表没有交点，返回`null`.
- 在返回结果后，两个链表仍须保持原有的结构。
- 可假定整个链表结构中没有循环。
- 程序尽量满足 $O(n)$ 时间复杂度，且仅用 $O(1)$ 内存。


## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：按正常思维都从头节点一一开始边遍历边比较，若其中某个指针已遍历到尾节点后，就指向另一个链表的头节点，最终有相交的肯定会使得`curA==curB`！若长度相等且不相交，遍历完第一轮后两指针都指向空即退出循环！

## AC代码：
```java
class Solution {
	public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
		if (headA == null || headB == null) // 注意判断输入输出的正确性！
			return null;
		ListNode curA = headA, curB = headB;
		while (curA != curB) {
			curA = curA == null ? headB : curA.next;
			curB = curB == null ? headA : curB.next;
		}
		return curA;
	}
}
```