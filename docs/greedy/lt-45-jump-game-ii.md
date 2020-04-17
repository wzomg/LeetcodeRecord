# 45.跳跃游戏II
题目链接：[传送门](https://leetcode-cn.com/problems/jump-game-ii/)

## 题目描述：
给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

**说明**:

假设你总是可以到达数组的最后一个位置。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：贪心。站在当前位置，先检查是否跳一步就能直接到达`len - 1`；若不能，则尝试从`1~nums[cur]`中选择下次能跳得最远距离的位置即可。 

## AC代码：
```java
class Solution {
	public int jump(int[] nums) {
		int len, maxRt, maxIdx, cur = 0, res = 0;
		if (nums == null || (len = nums.length) == 0)
			return 0;
		while (cur < len - 1) { // cur 表示当前所处位置
			// 先检查一下当前位置跳一步是否可到达 len - 1，若是则跳一步即可
			if (cur + nums[cur] >= len - 1) {
				res++;
				break;
			}
			// 否则继续从当前位置向右在连续 nums[cur] 个元素中找到能跳得最远距离的位置
			maxIdx = cur + 1; // 至少跳1步到下一个位置
			maxRt = cur + 1 + nums[cur + 1];
			for (int i = 2; i <= nums[cur]; i++) { // 从2步开始
				if (cur + i + nums[cur + i] > maxRt) {
					maxRt = cur + i + nums[cur + i];
					maxIdx = cur + i;
				}
				if (maxRt >= len - 1) // 优化
					break;
			}
			res++;
			// 选择下次能跳得最远的位置
			cur = maxIdx;
		}
		return res;
	}
}
```
