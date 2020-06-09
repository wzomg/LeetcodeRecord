# 面试题46.把数字翻译成字符串
题目链接：[传送门](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

## 题目描述：
给定一个数字，我们按照如下规则把它翻译为字符串：`0`翻译成`“a”`，`1`翻译成`“b”`，$\cdots$，`11`翻译成`“l”`，$\cdots$，`25`翻译成`“z”`。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

**提示**：$0 \leq$ `num` $< 2^{31}$

**示例**:
- 输入: `12258`
- 输出: `5`
- 解释: `12258`有`5`种不同的翻译，分别是`"bccfi", "bwfi", "bczi", "mcfi"`和`"mzi"`

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n)$
- 思路：记忆化搜索，分两种情况：翻译一个字符；翻译2个字符！

## AC代码：
```java
class Solution {
	private int[] dp;
	public int translateNum(int num) {
		String str = String.valueOf(num);
		int len = str.length();
		dp = new int[len + 1];
		return dfs(str, 0, len);
	}
	private int dfs(String num, int cur, int len) {
		if (cur == len)
			return 1;
		if (dp[cur] > 0) // 记忆化搜索
			return dp[cur];
		// 翻译一个字符
		int res = dfs(num, cur + 1, len);
		// 同时翻译两个字符，要注意这两个字符组成的数字之和不小于10且不超过25
		// 为什么要满足不小于10呢？因为若第一位为0，则结果会重复统计一些相同的情况；若加个判断这2位不小于10，就不会重复统计！
		if (cur + 1 < len) {
			String tmp = num.substring(cur, cur + 2);
			if ("10".compareTo(tmp) <= 0 && "25".compareTo(tmp) >= 0)
				res += dfs(num, cur + 2, len);
		}
		return dp[cur] = res;
	}
}
```