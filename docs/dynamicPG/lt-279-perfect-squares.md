# 279.完全平方数
题目链接：[传送门](https://leetcode-cn.com/problems/perfect-squares/)

## 题目描述：
给定正整数`n`，找到若干个完全平方数（比如 $1, 4, 9, 16, \dots $）使得它们的和等于`n`。你需要让组成和的完全平方数的个数最少。

**示例**:

- 输入: `n = 12`
- 输出: `3` 
- 解释: `12 = 4 + 4 + 4`

## 解决方案：
- 时间复杂度：$O(n \times \sqrt{n})$
- 空间复杂度：$O(n)$
- 思路：完全背包。定义`dp[i]`为组成`i`使用完全平方数的最少个数。

## AC代码：
```java
class Solution {
	public int numSquares(int n) {
		int[] dp = new int[n + 1];
		Arrays.fill(dp, Integer.MAX_VALUE - 10);
		dp[0] = 0; // 使用0个完全平方数构成元素0
		// 只需枚举 根号 n 个数即可
		for (int i = 1; i * i <= n; i++) {
			for (int j = i * i; j <= n; j++) // 正序枚举，因为每个完全平方数可重复被使用
				dp[j] = Math.min(dp[j], dp[j - i * i] + 1);
		}
		return dp[n];
	}
}
```