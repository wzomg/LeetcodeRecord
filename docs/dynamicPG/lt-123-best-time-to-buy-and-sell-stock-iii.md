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
- 思路：动态规划。状态转移看代码注释！

## AC代码：
```java
class Solution {
    public int maxProfit(int[] prices) {
        int len;
        if (prices == null || (len = prices.length) == 0) return 0;
        //只进行过第一次和第二次买操作情况下获得的利润为 -prices[0]
        int buy1 = -prices[0], sell1 = 0;
        int buy2 = -prices[0], sell2 = 0;
        for (int i = 1; i < len; i++) {
            buy1 = Math.max(buy1, -prices[i]); // 只进行过一次买操作获得的最大利润
            sell1 = Math.max(sell1, buy1 + prices[i]); //进行了一次买操作和一次卖操作获得的最大利润
            buy2 = Math.max(buy2, sell1 - prices[i]);//在完成了一笔交易的前提下，进行了第二次买操作获得的最大利润
            sell2 = Math.max(sell2, buy2 + prices[i]);//完成了全部两笔交易获得最大利润
        }
        return sell2;
    }
}
```