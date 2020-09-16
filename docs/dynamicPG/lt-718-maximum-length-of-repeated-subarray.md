# 718.最长重复子数组
题目链接：[传送门](https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/)

## 题目描述：
给两个整数数组`A`和`B`，返回两个数组中公共的、长度最长的子数组的长度。

**提示**：

- $ 1 \leq $ `len(A)`, `len(B)` $ \leq 1000 $
- $ 0 \leq $ `A[i]`, `B[i]` $ < 100 $

**示例**：

- 输入：A: `[1,2,3,2,1]`，`B: [3,2,1,4,7]`
- 输出：3
- 解释：长度最长的公共子数组是`[3, 2, 1]`。

## 解决方案：
- 时间复杂度：$O(n \times m)$
- 空间复杂度：$O(n \times m)$
- 思路：简单动态规划。本质是求最长公共子串的长度。

## AC代码：
```java
class Solution {
	public int findLength(int[] A, int[] B) {
		int len1, len2, res = 0;
		if (A == null || B == null || (len1 = A.length) == 0 || (len2 = B.length) == 0)
			return 0;
		int[][] dp = new int[len1 + 1][len2 + 1];
		for (int i = 1; i <= len1; i++) {
			for (int j = 1; j <= len2; j++) {
				if (A[i - 1] == B[j - 1]) {
					dp[i][j] = dp[i - 1][j - 1] + 1;
					res = Math.max(res, dp[i][j]);
				} else
					dp[i][j] = 0;
			}
		}
		return res;
	}
}
```