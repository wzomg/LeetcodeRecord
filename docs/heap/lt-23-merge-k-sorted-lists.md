# 23.合并K个排序链表
题目链接：[传送门](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

## 题目描述：
合并`k`个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

**示例**：
- 输入:
`[
  1->4->5,
  1->3->4,
  2->6
]`
- 输出: `1->1->2->3->4->4->5->6`

## 解决方案：
- 时间复杂度：$O(N × log^k)$，`N`为所有链表的节点总个数。
- 空间复杂度：$O(N)$
- 思路：建立一个最小堆来实现即可。注意：链表头节点判空！

## AC代码：
```java
class Solution {
	public ListNode mergeKLists(ListNode[] lists) {
		int len;
		ListNode res = new ListNode(0), small, cur = res;
		if (lists == null || (len = lists.length) == 0)
			return null;
		// 维护一个最小堆，其会根据每个链表的头节点的值进行比较排序
		Queue<ListNode> queue = new PriorityQueue<ListNode>(new Comparator<ListNode>() {
			@Override
			public int compare(ListNode o1, ListNode o2) {
				return o1.val - o2.val;
			}
		});
		for (int i = 0; i < len; i++) {
			if (lists[i] != null)
				queue.add(lists[i]);
		}
		while (!queue.isEmpty()) {
			small = queue.poll(); // 将整个链表出队
			cur.next = small; // 避免的申请节点所花费的时间
			if (small.next != null)
				queue.add(small.next);
			cur = cur.next;
		}
		return res.next;
	}
}
```