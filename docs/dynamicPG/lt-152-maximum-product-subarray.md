# 152.乘积最大子数组
题目链接：[传送门](https://leetcode-cn.com/problems/maximum-product-subarray/)

## 题目描述：
给你一个整数数组`nums`，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

**示例**:

- 输入: `[2,3,-2,4]`
- 输出: `6`
- 解释: 子数组`[2,3]`有最大乘积`6`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：动态规划。维护一个当前最大连续子序列的乘积和一个当前最小连续子序列的乘积，因为负数的绝对值较大的再乘以一个负数可能变成最大值！

## AC代码：
```java
class Solution {
	public int maxProduct(int[] nums) {
		int len, res;
		if (nums == null || (len = nums.length) == 0)
			return 0;
		int[] pludDp = new int[len];
		int[] minsDp = new int[len];
		res = pludDp[0] = minsDp[0] = nums[0];
		for (int i = 1; i < len; i++) {
			pludDp[i] = Math.max(Math.max(pludDp[i - 1] * nums[i], minsDp[i - 1] * nums[i]), nums[i]);
			minsDp[i] = Math.min(Math.min(pludDp[i - 1] * nums[i], minsDp[i - 1] * nums[i]), nums[i]);
			res = Math.max(res, pludDp[i]);
		}
		return res;
	}
}
```