# 516.最长回文子序列
题目链接：[传送门](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)

## 题目描述：
给你一个字符串`s`，找出其中最长的回文子序列，并返回该序列的长度。

子序列定义为：不改变剩余字符顺序的情况下，删除某些字符或者不删除任何字符形成的一个序列。

**提示**：

- $1 \leq $ `s.length` $ \leq 1000$
- s 仅由小写英文字母组成

**示例**：

- 输入：`s = "bbbab"`
- 输出：4
- 解释：一个可能的最长回文子序列为`"bbbb"`。

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n^2)$
- 思路：动态规划。`dp[i][j]`：表示字符串下标所在区间`[i,j]`构成连续子串的最大回文串的长度，其中 $i < j$。由此可得状态转移方程：当`s[i] == s[j]`时，`dp[i][j] = dp[i + 1][j - 1] + 2`；当`s[i] != s[j]`时，`dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])`，最后取`dp[0][len-1]`即可。

## AC代码：
```go
func longestPalindromeSubseq(s string) int {
	sLen := len(s)
	dp := make([][]int, sLen)
	for i := 0; i < sLen; i++ {
		dp[i] = make([]int, sLen)
		dp[i][i] = 1 //一个字符也是回文串
	}
	for i := sLen - 1; i >= 0; i-- { //枚举左端点
		for j := i + 1; j < sLen; j++ { //枚举右端点
			if s[i] == s[j] {
				dp[i][j] = dp[i+1][j-1] + 2
			} else {
				dp[i][j] = max(dp[i][j-1], dp[i+1][j]) //取最大，可以向左右两边扩展，取最大
			}
		}
	}
	return dp[0][sLen-1]
}
func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```