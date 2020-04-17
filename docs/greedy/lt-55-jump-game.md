# 55.跳跃游戏
题目链接：[传送门](https://leetcode-cn.com/problems/jump-game/)

## 题目描述：
给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：贪心。每次维护之前能达到的最远位置，若小于当前位置，则一定不能到达终点，否则就更新当前能跳的最远距离。

## AC代码：
```java
class Solution {
	public boolean canJump(int[] nums) {
		int maxVal = 0, len;
		boolean flag = true;
		if ((len = nums.length) < 2)
			return flag;
		for (int i = 0; i < len; i++) {
			if (maxVal < i) {
				flag = false;
				break;
			}
			maxVal = Math.max(maxVal, i + nums[i]);
		}
		return flag;
	}
}
```
