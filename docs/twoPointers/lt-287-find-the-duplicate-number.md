# 287.寻找重复数
题目链接：[传送门](https://leetcode-cn.com/problems/find-the-duplicate-number/)

## 题目描述：
给定一个包含 $n + 1$ 个整数的数组`nums`，其数字都在`1`到`n`之间（包括`1`和`n`），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

**示例**:
- 输入: `[1,3,4,2,2]`
- 输出: `2`

**说明**：

- 不能更改原数组（假设数组是只读的）。
- 只能使用额外的 $O(1)$ 的空间。
- 时间复杂度小于 $O(n^2)$。
- 数组中只有一个重复的数字，但它可能不止重复出现一次。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：快慢指针。因为数组元素保证是 $1 \sim n$，并且数组长度为 $n+1$，且至少存在一个重复元素，所以我们可以像查询循环单链表那样找到环的入口节点即可。

## AC代码：
```java
class Solution {
	public int findDuplicate(int[] nums) {
		int slow = 0, fast = 0, rcur = 0;
		do {
			slow = nums[slow];
			fast = nums[nums[fast]];
		} while (slow != fast);
		while (rcur != slow) {
			rcur = nums[rcur];
			slow = nums[slow];
		}
		return rcur;
	}
}
```