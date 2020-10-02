# LCP19.秋叶收藏集
题目链接：[传送门](https://leetcode-cn.com/problems/UlBDOe/)

## 题目描述：
小扣出去秋游，途中收集了一些红叶和黄叶，他利用这些叶子初步整理了一份秋叶收藏集`leaves`， 字符串`leaves`仅包含小写字符`r`和`y`， 其中字符`r`表示一片红叶，字符`y`表示一片黄叶。
出于美观整齐的考虑，小扣想要将收藏集中树叶的排列调整成「红、黄、红」三部分。每部分树叶数量可以不相等，但均需大于等于`1`。每次调整操作，小扣可以将一片红叶替换成黄叶或者将一片黄叶替换成红叶。请问小扣最少需要多少次调整操作才能将秋叶收藏集调整完毕。

**提示**：

- $3 \leq$ `leaves.length` $ \leq 10^5$；
- `leaves`中只包含字符`'r'`和字符`'y'`。

**示例1**：

- 输入：`leaves = "rrryyyrryyyrr"`
- 输出：`2`
- 解释：调整两次，将中间的两片红叶替换成黄叶，得到`"rrryyyyyyyyrr"`。

**示例2**：

- 输入：`leaves = "ryr"`
- 输出：`0`
- 解释：已符合要求，不需要额外操作。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：动态规划。定义`dp[i][j]`表示前`i+1`片叶子处于`j`状态时的最小操作次数，`j=0`（左半部分：红色），`j=1`（中间部分：黄色），`j=2`（右半部分：红色）。状态转移详解看代码注释。

## AC代码：
```java
class Solution {
	public int minimumOperations(String leaves) {
		int len;
		if (leaves == null || (len = leaves.length()) == 0)
			return 0;
		int[][] dp = new int[len][3];
		// 唯一确定的初始状态，若第一片叶子为黄色，则需要变成红色
		dp[0][0] = leaves.charAt(0) == 'y' ? 1 : 0;
		// dp[0][1] 和 dp[0][2] （只有一片叶子） 的状态是无效的
		// dp[1][2] （只有2片叶子） 的状态是无效的
		// 因为状态转移是取最小值，所以初始的无效状态需要设置成最大值
		dp[0][1] = dp[0][2] = dp[1][2] = Integer.MAX_VALUE;
		for (int i = 1, isRed, isYellow; i < len; i++) {
			isRed = leaves.charAt(i) == 'r' ? 1 : 0;
			isYellow = leaves.charAt(i) == 'y' ? 1 : 0;
			// 当j == 0时，只能是左半部分为红色
			dp[i][0] = dp[i - 1][0] + isYellow;
			// 当j == 1时，第 i - 1 片叶子可能为红色或黄色，取2者中的最小值，并且当前叶子要变成黄色
			dp[i][1] = Math.min(dp[i - 1][0], dp[i - 1][1]) + isRed;
			// j == 2时，第 i - 1 片叶子可能处于黄色或红色，取2者中的最小值，并且当前叶子要变成红色
			// 前提是叶子的数量至少为3才能进行状态转移
			if (i >= 2)
				dp[i][2] = Math.min(dp[i - 1][1], dp[i - 1][2]) + isYellow;
		}
		// 取右半部分状态为2时的最大值
		return dp[len - 1][2];
	}
}
```