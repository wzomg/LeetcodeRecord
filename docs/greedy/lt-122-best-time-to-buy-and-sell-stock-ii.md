# 122.买卖股票的最佳时机 II
题目链接：[传送门](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

## 题目描述：
给定一个数组，它的第`i`个元素是一支给定股票第`i`天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**提示**：

- $ 1 \leq $ `prices.length` $ \leq 3 \times 10^4 $
- $ 0 \leq $ `prices[i]` $ \leq 10^4 $

**示例**： 

- 输入: `[7,1,5,3,6,4]`
- 输出: `7`
- 解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。



## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：贪心或动态规划。贪心：从前往后遍历数组可以发现，我们要买入的一天和要卖出的那天构成的区间恰好是升序区间，而这两个元素的差值正是这个区间中相邻元素之间的差值之和。因此，我们可以分别累加多个升序区间中各个相邻元素的差值之和即为最优答案！
动态规划：定义`dp[i][0,1]`，`0`表示第`i`天不持股票的最大收益，`1`表示第`i`天持有股票的最大收益，状态转移方程详见代码注释！

## AC代码1：贪心。
```java
class Solution {
    public int maxProfit(int[] prices) {
        int len, res = 0;
        if(prices == null || (len = prices.length) == 0) return 0;
        for(int i = 1; i < len; i++) 
            if(prices[i] > prices[i - 1]) res += prices[i] - prices[i - 1];
        return res;
    }
}
```

## AC代码2：动态规划。
```java
class Solution {
	public int maxProfit(int[] prices) {
		int len;
		if (prices == null || (len = prices.length) == 0)
			return 0;
		int[][] dp = new int[len][2];
		dp[0][0] = 0;
		dp[0][1] = -prices[0];
		for (int i = 1; i < len; i++) {
			// 第i-1天不持有股票，第i天仍不持有股票，也就是不买
			// 第i-1天持有股票，第i天不持有股票，即卖出
			dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
			// 第i-1天持有股票，第i天仍持有股票，也就是不卖
			// 第i-1天不持有股票，第i天持有股票，即买入
			dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
		}
		return dp[len - 1][0]; // 返回不持有股票的最大收益
	}
}
```
