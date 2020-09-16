# 121.买卖股票的最佳时机
题目链接：[传送门](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

## 题目描述：
给定一个数组，它的第 `i` 个元素是一支给定股票第 `i` 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

**注意**：你不能在买入股票前卖出股票。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：低价买入，高价卖出。我们知道，从左边找到一个较小值，就得从其后面找到一个最大值！于是我们可以维护一个前缀最小值买入点，买入点之后都是卖出点的候选点，边遍历边取最大利润即可！

## AC代码：
```java
class Solution {
	public int maxProfit(int[] prices) {
		int len, minPrice, res = 0;
		if (prices == null || (len = prices.length) == 0)
			return res;
		minPrice = prices[0];
		for (int i = 1; i < len; i++) {
			minPrice = Math.min(prices[i], minPrice);
			res = Math.max(res, prices[i] - minPrice);
		}
		return res;
	}
}
```