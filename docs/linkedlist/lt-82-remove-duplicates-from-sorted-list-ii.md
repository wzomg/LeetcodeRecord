# 82.删除排序链表中的重复元素 II
题目链接：[传送门](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

## 题目描述：
给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中**没有重复出现**的数字。

**示例**：

- 输入: `1->2->3->3->4->4->5`
- 输出: `1->2->5`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：简单单链表操作，操作前虚拟一个头节点，时刻记录`cur`的前一个节点`pre`，这样增删改比较方便，实际在遍历过程中，`pre`指针就将正确的节点链接起来了！

## AC代码：
```java
class Solution {
	public ListNode deleteDuplicates(ListNode head) {
		ListNode dummyHead = new ListNode(-1); // 虚拟一个头节点
		dummyHead.next = head;
		ListNode pre = dummyHead, cur = dummyHead.next, tmp;
		boolean flag;
		while (cur != null) {
			flag = false;
			while (cur.next != null && cur.val == cur.next.val) {
				flag = true;
				cur.next = cur.next.next;
			}
			if (flag) { // 删除当前节点
				// 老老实实先将剩下的链表头节点拿出来
				tmp = cur.next;
                // 注意：要将前一个节点的next指向下一个节点，恢复真正主链上节点的链接
				pre.next = tmp;
                // 移动指针归位
				cur = tmp;
				continue;
			}
            // pre 指针时刻指向当前节点
			pre = cur;
            // 移动指针下移
			cur = cur.next;
		}
		return dummyHead.next;
	}
}
```