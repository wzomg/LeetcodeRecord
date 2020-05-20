# 面试题14-I.剪绳子
题目链接：[传送门](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

## 题目描述：
给你一根长度为`n`的绳子，请把绳子剪成整数长度的`m`段（`m`、`n`都是整数，$n>1$并且 $m>1$），每段绳子的长度记为 $k[0],k[1] \cdots k[m-1]$。请问 $k[0] \times k[1] \times \cdots \times k[m-1]$ 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

**提示**：

- $ 2 \leq n \leq 58 $

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n)$
- 思路：记忆化搜索，枚举每个切点，递归取最大值！

## AC代码：
```java
class Solution {
	private int[] dp;
	public int cuttingRope(int n) {
		dp = new int[n + 1];
		return dfs(n);
	}
	private int dfs(int n) {
		if (n <= 2) // 长度为2至少切割成1+1，即1*1=1
			return 1;
		if (dp[n] > 0) // 表示当前线段已计算完成
			return dp[n];
		int res = 0;
		for (int i = 1, len = n + 1 >> 1; i <= len; i++) // 根据对称性，只需枚举前一半的切点即可，因为n-i大的递归切割对答案的贡献可能变大
			// 因为题目要求至少切割成2部分，所以这条线段至少切成2部分：i * (n - i)，可能有更多：i * dfs(n - i)
			res = Math.max(res, Math.max(i * (n - i), i * dfs(n - i)));
		return dp[n] = res; // 记忆化搜索
	}
}
```