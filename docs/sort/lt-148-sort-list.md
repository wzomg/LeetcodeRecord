# 148.排序链表
题目链接：[传送门](https://leetcode-cn.com/problems/sort-list/)

## 题目描述：
在 $O(n \times log^n)$ 时间复杂度和常数级空间复杂度下，对链表进行排序。

**示例**：

- 输入：`4->2->1->3`
- 输出：`1->2->3->4`

## 解决方案：
- 时间复杂度：$O(n × log^n)$
- 空间复杂度：$O(1)$
- 思路：模拟归并排序：自底向上逐一合并有序链表。

## AC代码：
```java
class Solution {
	public ListNode sortList(ListNode head) {
		ListNode dummyHead = new ListNode(-1); // 创建一个虚拟头节点
		ListNode cur = dummyHead.next = head, tail, left, right;
		int len = 0;
		// 先计算链表长度
		while (cur != null) {
			len++;
			cur = cur.next;
		}
		// 然后循环切割、同时合并
		// 模拟归并排序：自底向上合并链表
		for (int siz = 1; siz < len; siz <<= 1) {
			// 当前层 链表的表头节点，已经排好序的表尾节点
			cur = dummyHead.next;
			tail = dummyHead;
			while (cur != null) {
				left = cur;
				// 切掉前siz个节点，剩下的给right
				right = cutLink(cur, siz);
				// 切掉前siz个节点，剩下的给cur
				cur = cutLink(right, siz);
				// 合并两个有序链表
				tail.next = merge(left, right);
				// tail指针时刻指向已合并后链表最后一个节点
				while (tail.next != null)
					tail = tail.next;
			}
		}
		// 返回虚拟头节点的下一个节点
		return dummyHead.next;
	}
	// 将链表切掉前n个节点，并返回第2部分链表的头节点
	private ListNode cutLink(ListNode head, int n) {
		ListNode cur = head, rightHead;
		// 只需循环n-1次到达尾节点
		while (--n > 0 && cur != null)
			cur = cur.next;
		// 若当前cur为空，则第2部分的头节点肯定为空
		if (cur == null)
			return null;
		// 保存下一个节点
		rightHead = cur.next;
		// 当前链表的尾节点指向的下一个节点置为null
		cur.next = null;
		return rightHead;
	}
	// 合并2个有序链表
	private ListNode merge(ListNode list1, ListNode list2) {
		ListNode dummyHead = new ListNode(-1), cur = dummyHead;
		while (list1 != null && list2 != null) {
			if (list1.val < list2.val) {
				// 无需 new 一个新的节点，直接指向下一个节点即可
				cur.next = list1;
				list1 = list1.next;
			} else {
				cur.next = list2;
				list2 = list2.next;
			}
			cur = cur.next;
		}
		// 指向剩下的节点
		cur.next = list1 == null ? list2 : list1;
		return dummyHead.next;
	}
}
```