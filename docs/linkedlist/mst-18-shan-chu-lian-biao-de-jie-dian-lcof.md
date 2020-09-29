# 面试题18.删除链表的节点
题目链接：[传送门](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

## 题目描述：
给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。返回删除后的链表的头节点。

**说明**：

- 题目保证链表中节点的值互不相同
- 若使用`C`或`C++`语言，你不需要`free`或`delete`被删除的节点

**示例**:

- 输入: `head = [4,5,1,9]`, `val = 5`
- 输出: `[4,1,9]`
- 解释: 给定你链表中值为5的第二个节点，那么在调用了你的函数之后，该链表应变为`4 -> 1 -> 9`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：双指针。用一个指针遍历到当前节点的值是否为val，移动的同时，用另外一个指针跟在其后面，这样删除的时候比较方便。

## AC代码：
```java
class Solution {
	public ListNode deleteNode(ListNode head, int val) {
		if (head.val == val)
			return head.next;
		else {
			ListNode pre = head, cur = head.next;
			while (cur != null && cur.val != val) {
				pre = pre.next;
				cur = cur.next;
			}
			if (cur != null && cur.val == val) // 若相同，则删之
				pre.next = cur.next;
			return head;
		}
	}
}
```