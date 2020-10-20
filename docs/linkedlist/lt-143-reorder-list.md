# 143.重排链表
题目链接：[传送门](https://leetcode-cn.com/problems/reorder-list/)

## 题目描述：
给定一个单链表`L`：`L0→L1→…→Ln-1→Ln`，将其重新排列后变为：`L0→Ln→L1→Ln-1→L2→Ln-2→…`

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

**示例1**：给定链表`1->2->3->4`，重新排列为`1->4->2->3`。

**示例2**：给定链表`1->2->3->4->5`，重新排列为`1->5->2->4->3`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：模拟题。先将节点存起来，然后首尾向中间靠拢相连。

## AC代码：
```java
class Solution {
	public void reorderList(ListNode head) {
		if (head == null)
			return;
		List<ListNode> nodes = new ArrayList<>();
		ListNode cur = head;
		while (cur != null) {
			nodes.add(cur);
			cur = cur.next;
		}
		int i = 0, j = nodes.size() - 1;
		while (i < j) {
			nodes.get(i).next = nodes.get(j);
			i++;
			if (i == j)
				break;
			nodes.get(j).next = nodes.get(i);
			j--;
		}
		nodes.get(i).next = null;
	}
}
```