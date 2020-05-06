# 983.最低票价
题目链接：[传送门](https://leetcode-cn.com/problems/minimum-cost-for-tickets/)

## 题目描述：
在一个火车旅行很受欢迎的国度，你提前一年计划了一些火车旅行。在接下来的一年里，你要旅行的日子将以一个名为`days`的数组给出。每一项是一个从`1`到`365`的整数。

火车票有三种不同的销售方式：

- 一张为期一天的通行证售价为`costs[0]`美元；
- 一张为期七天的通行证售价为`costs[1]`美元；
- 一张为期三十天的通行证售价为`costs[2]`美元。

通行证允许数天无限制的旅行。例如，如果我们在第 2 天获得一张为期 7 天的通行证，那么我们可以连着旅行 7 天：第 2 天、第 3 天、第 4 天、第 5 天、第 6 天、第 7 天和第 8 天。

返回你想要完成在给定的列表`days`中列出的每一天的旅行所需要的最低消费。

**提示**：

- $1 \leq $ `days.length` $ \leq 365$
- $1 \leq $`days[i]` $\leq 365$
- `days`按顺序严格递增
- `costs.length` $== 3$
- $1 \leq$ `costs[i]` $ \leq 1000 $

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：记忆化搜索+二分查找。记忆化常规套路，在第`day[i]`天处分别买`1`或`7`或`30`天的通行证所需花费的最小值，并且记忆化每个`i`处的最小费用即可。

## AC代码：
```java
class Solution {
	private int[] dp;
	private int[] dir = new int[] { 1, 7, 30 };
	public int mincostTickets(int[] days, int[] costs) {
		int len;
		if (days == null || (len = days.length) == 0)
			return 0;
		dp = new int[len];
		// 初始值都为-1
		for (int i = 0; i < len; i++)
			dp[i] = -1;
		return dfs(days, costs, 0, len);
	}
	private int dfs(int[] days, int[] costs, int pos, int len) {
		if (pos >= len)
			return 0;
		if (dp[pos] > -1)
			return dp[pos];
		int res = Integer.MAX_VALUE; // 注意：这里要设置为最大值
		// 遍历取3种情况的最小值
		for (int i = 0, tmp, index; i < costs.length; i++) {
			index = upperBound(pos, len - 1, days, days[pos] + dir[i] - 1);
			tmp = dfs(days, costs, index, len) + costs[i];
			res = Math.min(res, tmp);
		}
		return dp[pos] = res; // 记忆化
	}
	// 找到一个大于 target元素的下标，没有则返回 days.length
	private int upperBound(int low, int high, int[] days, int target) {
		int mid, res = -1;
		while (low <= high) {
			mid = (low + high) >> 1;
			if (days[mid] > target) {
				high = mid - 1;
				res = mid;
			} else
				low = mid + 1;
		}
		return res == -1 ? days.length : res;
	}
}
```
