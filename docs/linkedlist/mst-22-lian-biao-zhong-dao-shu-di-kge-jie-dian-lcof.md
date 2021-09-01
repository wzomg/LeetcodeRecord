# 面试题22.链表中倒数第k个节点
题目链接：[传送门](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

## 题目描述：
输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是`1、2、3、4、5、6`。这个链表的倒数第3个节点是值为4的节点。

**示例**：

- 给定一个链表：`1->2->3->4->5`, 和`k = 2`。
- 返回链表：`4->5`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：双指针移动。先定位一个指针到第k个节点的位置，然后和另外一个指针从头开始一起移动，直到指针cur2移动到最后一个节点为止！

## AC代码：
- Java

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) {
        val = x;
    }
}
class Solution {
	public ListNode getKthFromEnd(ListNode head, int k) {
		ListNode cur1 = head, cur2 = head;
		while (--k > 0 && cur2 != null)
			cur2 = cur2.next;
		if (cur2 == null) // 若cur2为空，说明链表没有k个节点，直接返回空
			return null;
        // cur2只需移动到最后一个节点的位置即可
		while (cur2.next != null) { 
			cur1 = cur1.next;
			cur2 = cur2.next;
		}
		return cur1;
	}
}
```

- Go

```go
type ListNode struct {
	Val  int
	Next *ListNode
}
func getKthFromEnd(head *ListNode, k int) *ListNode {
	cur := head
	for ; k > 0; k-- {
		if cur == nil {
			return nil
		}
		cur = cur.Next
	}
	pre := head
	for cur != nil {
		pre = pre.Next
		cur = cur.Next
	}
	return pre
}
```