# 面试题06.从尾到头打印链表
题目链接：[传送门](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

## 题目描述：
输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

**限制**：$0 \leq $ 链表长度 $ \leq 10000 $

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：递归到链表尾，在递归出口条件中记录链表长度并创建数组，回溯时对数组进行反向填充即可。

## AC代码：
```java
class Solution {
	private int len;
	private int[] nums;
	public int[] reversePrint(ListNode head) {
		len = 0;
		dfs(head, 0);
		return nums;
	}
	private void dfs(ListNode cur, int pos) {
		if (cur == null) {
			nums = new int[len = pos];
			return;
		}
		dfs(cur.next, pos + 1);
		nums[len - 1 - pos] = cur.val; // 反向填充
	}
}
```
