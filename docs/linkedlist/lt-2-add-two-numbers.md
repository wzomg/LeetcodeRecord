# 2.两数相加
题目链接：[传送门](https://leetcode-cn.com/problems/add-two-numbers/)

## 题目描述：
给出两个**非空**的链表用来表示两个非负的整数。其中，它们各自的位数是按照**逆序**的方式存储的，并且它们的每个节点只能存储**一位**数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字`0`之外，这两个数都不会以`0`开头。

**示例**：

- 输入：`(2 -> 4 -> 3) + (5 -> 6 -> 4)`
- 输出：`7 -> 0 -> 8`
- 原因：$ 342 + 465 = 807 $

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：小学数学，只需按位相加，最后的进位单独处理！虚拟一个头指针指向答案整条链的首地址，通过另一个指针cur创建new下一个next节点，注意：要先new创建当前指针的下一个next节点再移动当前cur到下一个节点的地址处，否则将会断开整条链！

## AC代码：
- Java：
```java
class Solution {
	public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
		ListNode dummyHead = new ListNode(-1), dummyCur = dummyHead;
		int carry = 0, now;
		ListNode newNode;
		while (l1 != null || l2 != null || carry > 0) {
			now = (l1 == null ? 0 : l1.val) + (l2 == null ? 0 : l2.val) + carry;
			carry = now / 10;
			if (l1 != null)
				l1 = l1.next;
			if (l2 != null)
				l2 = l2.next;
			newNode = new ListNode(now % 10);
			dummyCur.next = newNode;
			dummyCur = dummyCur.next;
		}
		return dummyHead.next;
	}
}
```
- Go：
```go
// go 不支持三目运算符
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
	dummyHead := new(ListNode)
	dummyCur := dummyHead
	carry := 0
	for l1 != nil || l2 != nil || carry != 0 {
		curSum := carry
		if l1 != nil {
			curSum += l1.Val
			l1 = l1.Next
		}
		if l2 != nil {
			curSum += l2.Val
			l2 = l2.Next
		}
		dummyCur.Next = new(ListNode)
		dummyCur = dummyCur.Next
		dummyCur.Val = curSum % 10
		carry = curSum / 10
	}
	return dummyHead.Next
}
```