# 面试题63.股票的最大利润
题目链接：[传送门](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)

## 题目描述：
假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？

## 示例1:
- 输入: `[7,1,5,3,6,4]`
- 输出: `5`
- 解释: 在第`2`天（股票价格`=1`）的时候买入，在第`5`天（股票价格`=6`）的时候卖出，最大利润`= 6-1=5`。注意利润不能是`7-1=6`, 因为卖出价格需要大于买入价格。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：动态规划思想。维护前`i-1`天股票的最小值，用第`i`天的股票价格减去最小值取个max即可。

## AC代码：
```java
class Solution {
	public int maxProfit(int[] prices) {
		int len, minVal, res = 0;
		if (prices == null || (len = prices.length) == 0)
			return 0;
		minVal = prices[0];
		for (int i = 1; i < len; i++) {
			res = Math.max(res, prices[i] - minVal);
			minVal = Math.min(minVal, prices[i]);
		}
		return res;
	}
}
```