# 188.买卖股票的最佳时机IV
题目链接：[传送门](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

## 题目描述：
给定一个数组，它的第`i`个元素是一支给定的股票在第`i`天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成`k`笔交易。

注意: 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例**:

- 输入: `[3,2,6,5,0,3]`, `k = 2`
- 输出: `7`
- 解释: 在第`2`天 (股票价格`= 2`) 的时候买入，在第`3`天 (股票价格`= 6`) 的时候卖出, 这笔交易所能获得利润`= 6-2 = 4`。随后，在第`5`天 (股票价格`= 0`) 的时候买入，在第`6`天 (股票价格`= 3`) 的时候卖出, 这笔交易所能获得利润`= 3-0 = 3`。 

## 解决方案：
- 时间复杂度：$O(n \times k)$
- 空间复杂度：$O(n)$
- 思路：动态规划。令`buy[i][j]`为前`i`支股票买卖`j`次（且最后一次为买入）可获得的最大利润；`sell[i][j]`为前`i`支股票买卖`j`次（且最后一次为卖出）可获得的最大利润。
    - 对于`buy[i][j]`来说，最后一次操作是**买入**，有两种情况：
        - 若不买第`i`支股票，则最大利润就是前`i-1`支股票买卖`j`次（且最后一次为**买入**）的最大利润：`buy[i][j] = buy[i-1][j]`；
        - 若买第`i`支股票，则最大利润就是前`i-1`支股票买卖`j-1`次（且最后一次为**卖出**）的最大利润：`buy[i][j] = sell[i-1][j-1]-prices[i]`。
    - 对于`sell[i][j]`来说，最后一次操作是**卖出**，有两种情况：
        - 若不买第`i`支股票，则最大利润就是前`i-1`支股票买卖`j`次（且最后一次为**卖出**）的最大利润：`sell[i][j] = sell[i-1][j]`；
        - 若买第`i`支股票，则最大利润就是前`i-1`支股票买卖`j`次（且最后一次为**买入**）的最大利润：`sell[i][j] = buy[i-1][j]+prices[i]`。
    - 综上所述，`buy[i][j] = max(buy[i-1][j], sell[i-1][j-1]-prices[i])`；<br>
    `sell[i][j] = max(sell[i-1][j], buy[i-1][j]+prices[i])`。
    - 注意：初始化`n=0`最后一次买入时的情况；当 $ k \geq \frac{len}{2} $ 时，可以直接用贪心法做。

## AC代码：
```java
class Solution {
	public int maxProfit(int k, int[] prices) {
		int len, sum = 0;
		if (prices == null || (len = prices.length) < 2)
			return 0;
		if (k >= len / 2) { // 只要k达到一半以上交易直接贪心取即可。
			for (int i = 1; i < len; i++)
				if (prices[i] > prices[i - 1])
					sum += prices[i] - prices[i - 1];
			return sum;
		}
		int[][] buy = new int[len][k + 1];
		int[][] sell = new int[len][k + 1];
		// 初始化第1支股票买卖j次（且最后一次为买入）可获得的最大利润为 -prices[0]，最坏情况下就买了一次，亏了 -prices[0]
		for (int j = 1; j <= k; j++)
			buy[0][j] = -prices[0];
		for (int i = 1; i < len; i++) { // 从第2支股票开始遍历
			for (int j = 1; j <= k; j++) { // 遍历买卖 1 - k 次
				buy[i][j] = Math.max(buy[i - 1][j], sell[i - 1][j - 1] - prices[i]);
				sell[i][j] = Math.max(sell[i - 1][j], buy[i - 1][j] + prices[i]);
			}
		}
        // 返回前len支股票买卖k次所得的最大利润
		return sell[len - 1][k];
	}
}
```