# 123.买卖股票的最佳时机III
题目链接：[传送门](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)

## 题目描述：
给定一个数组，它的第`i`个元素是一支给定的股票在第`i`天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成**两笔**交易。

**注意**: 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例**:

- 输入: `[3,3,5,0,0,3,1,4]`
- 输出: `6`
- 解释: 在第`4`天（股票价格`= 0`）的时候买入，在第`6`天（股票价格`= 3`）的时候卖出，这笔交易所能获得利润`= 3-0 = 3`。随后，在第`7`天（股票价格`= 1`）的时候买入，在第`8`天 （股票价格`= 4`）的时候卖出，这笔交易所能获得利润`= 4-1 = 3`。 

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：动态规划。定义：`dp1[i] = max(dp1[i-1], prices[i] - minVal)`从前往后遍历，表示第1天到第i天中某两个值产生的最大利润（通过是否在第i天**第一次**卖出确认）；`dp2[i] = max(dp2[i+1], maxVal - prices[i])`从后往前遍历，表示第i天到最后一天中某两个值产生的最大利润（通过是否在第i天**第二次**买进确认）；`res = max(dp1[i] + dp2[i])`，表示从第1天到最后一天经过2次交易产生的最大利润。注：在同一天先卖出再买入相当于合并两个子区间为一个大区间就买一次而已。

## AC代码：
```java
class Solution {
	public int maxProfit(int[] prices) {
		int len;
		if (prices == null || (len = prices.length) < 2)
			return 0;
		int[] dp1 = new int[len];
		int[] dp2 = new int[len];
		int minVal = prices[0], maxVal = prices[len - 1], res = 0;
		for (int i = 1; i < len; i++) { // 从第二个开始遍历
			dp1[i] = Math.max(dp1[i - 1], prices[i] - minVal);
			minVal = Math.min(minVal, prices[i]);
		}
		for (int i = len - 2; i >= 0; i--) { // 从倒数第二个开始遍历
			dp2[i] = Math.max(dp2[i + 1], maxVal - prices[i]);
			maxVal = Math.max(maxVal, prices[i]);
		}
		for (int i = 0; i < len; i++)
			res = Math.max(res, dp1[i] + dp2[i]);
		return res;
	}
}
```
