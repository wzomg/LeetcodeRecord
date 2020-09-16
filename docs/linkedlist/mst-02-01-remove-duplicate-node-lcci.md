# 面试题 02.01. 移除重复节点
题目链接：[传送门](https://leetcode-cn.com/problems/remove-duplicate-node-lcci/)

## 题目描述：
编写代码，移除未排序链表中的重复节点。保留最开始出现的节点。

**示例**:

- 输入：`[1, 2, 3, 3, 2, 1]`
- 输出：`[1, 2, 3]`

**提示**：

- 链表长度在`[0, 20000]`范围内。
- 链表元素在`[0, 20000]`范围内。

**进阶**：

如果不得使用临时缓冲区，该怎么解决？

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：哈希表+双指针。简单处理即可！

## AC代码：
```java
class Solution {
	public ListNode removeDuplicateNodes(ListNode head) {
		Map<Integer, Boolean> map = new HashMap<Integer, Boolean>();
		ListNode dummyHead = new ListNode(-1);
		dummyHead.next = head;
		ListNode cur = dummyHead.next, pre = dummyHead, tmp;
		while (cur != null) {
			// 有重复就删除
			if (map.containsKey(cur.val)) {
				// 老老实实先将剩下的链表头节点拿出来
				tmp = cur.next;
				// 注意：要将前一个节点的next指向下一个节点
				pre.next = tmp;
				// 移动指针归位
				cur = tmp;
				continue;
			}
			map.put(cur.val, true);
			pre = cur;
			cur = cur.next;
		}
		return dummyHead.next;
	}
}
```