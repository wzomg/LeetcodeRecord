# 876.链表的中间结点
题目链接：[传送门](https://leetcode-cn.com/problems/middle-of-the-linked-list/)

## 题目描述：
给定一个带有头结点`head`的非空单链表，返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。

**提示**：给定链表的结点数介于 $1$ 和 $100$ 之间。

**示例**：
- 输入：`[1,2,3,4,5]`
- 输出：此列表中的结点 3 (序列化形式：[3,4,5])，返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
- 注意：我们返回了一个`ListNode`类型的对象`ans`，这样：`ans.val = 3`, `ans.next.val = 4`, `ans.next.next.val = 5`, 以及 `ans.next.next.next = NULL`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：快慢指针。分奇、偶情况在草稿纸上稍微模拟一下即可出答案！

## AC代码：
```java
class Solution {
	public ListNode middleNode(ListNode head) {
		if (head == null)
			return null;
		ListNode slow = head, fast = head;
		while (fast != null && fast.next != null) {
			slow = slow.next;
			fast = fast.next.next;
		}
		return slow;
	}
}
```