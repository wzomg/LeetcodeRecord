# 1262.可被三整除的最大和
题目链接：[传送门](https://leetcode-cn.com/problems/greatest-sum-divisible-by-three/)

## 题目描述：
给你一个整数数组`nums`，请你找出并返回能被三整除的元素最大和。

**提示**：

- $1 \leq $ `nums.length` $ \leq 4 \times 10^4 $
- $1 \leq $ `nums[i]` $ \leq 10^4 $

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：动态规划。定义`dp[i][0,1,2]`表示前`i`个数中某些数相加模`3`余数为`0`，`1`，`2`的最大和。注意：初始化非法状态为**负无穷小**。

## AC代码：
```java
class Solution {
	public int maxSumDivThree(int[] nums) {
		int len;
		if (nums == null || (len = nums.length) == 0)
			return 0;
		int[][] dp = new int[len + 1][3];
		// 定义合法状态初始最大和为0： dp[0][0] = 0
		dp[0][0] = 0;
		// 表示非法状态，即模3余1或模3余2的最大和为负无穷小
		dp[0][1] = dp[0][2] = Integer.MIN_VALUE;
		for (int i = 1, res; i <= len; i++) {
			res = nums[i - 1] % 3;
			if (res == 0) {
				dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][0] + nums[i - 1]); // 0, 0
				dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][1] + nums[i - 1]); // 0, 1 
				dp[i][2] = Math.max(dp[i - 1][2], dp[i - 1][2] + nums[i - 1]); // 0, 2
			} else if (res == 1) {
				dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][2] + nums[i - 1]); // 1, 2
				dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] + nums[i - 1]); // 1, 0
				dp[i][2] = Math.max(dp[i - 1][2], dp[i - 1][1] + nums[i - 1]); // 1, 1
			} else { // 2
				dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + nums[i - 1]); // 2, 1
				dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][2] + nums[i - 1]); // 2, 2
				dp[i][2] = Math.max(dp[i - 1][2], dp[i - 1][0] + nums[i - 1]); // 2, 0
			}
		}
		// 返回前 i 个数中某些数相加模3余0的最大和
		return dp[len][0];
	}
}
```
