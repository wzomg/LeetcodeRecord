# 322.零钱兑换
题目链接：[传送门](https://leetcode-cn.com/problems/coin-change/)

## 题目描述：
给定不同面额的硬币`coins`和一个总金额`amount`。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回`-1`。

**说明**：

你可以认为每种硬币的数量是无限的。

## 解决方案：
- 时间复杂度：$O(n × M)$
- 空间复杂度：$O(M)$
- 思路：完全背包入门题。定义`dp[i]`为组成和为`i`使用硬币的最少个数。先初始化`dp`数组元素值为无穷大`INF`，其中`dp[0] = 0`，然后正向枚举，每次从`j-coins[i]`状态转移过来，即`dp[j]=min(dp[j-coins[i]]+1,dp[j])`，最后判断`dp[M]`是否小于`INF`即可！

## AC代码：
```java
class Solution {
	public int coinChange(int[] coins, int amount) {
		int len;
		if (coins == null || (len = coins.length) == 0)
			return -1;
		int[] dp = new int[amount + 1];
		Arrays.fill(dp, 0x3f3f3f3f);
		dp[0] = 0; // 组成金额为0需要用0个硬币
		for (int i = 0; i < len; i++) {
			for (int j = coins[i]; j <= amount; j++) // 正序枚举，以重复使用元素
				dp[j] = Math.min(dp[j], dp[j - coins[i]] + 1);
		}
		return dp[amount] < 0x3f3f3f3f ? dp[amount] : -1;
	}
}
```