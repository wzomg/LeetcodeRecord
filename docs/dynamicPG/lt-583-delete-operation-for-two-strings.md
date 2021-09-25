# 583.两个字符串的删除操作
题目链接：[传送门](https://leetcode-cn.com/problems/delete-operation-for-two-strings/)

## 题目描述：
给定两个单词 word1 和 word2，找到使得 word1 和 word2 相同所需的最小步数，每步可以删除任意一个字符串中的一个字符。

**提示**：
- 给定单词的长度不超过500。
- 给定单词中的字符只含有小写字母。

**示例**：
- 输入: "sea", "eat"
- 输出: 2
- 解释: 第一步将"sea"变为"ea"，第二步将"eat"变为"ea"

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n^2)$
- 思路：典型地求最长公共子序列，这样才会使得删除的次数最少。

## AC代码：
```go
func minDistance(word1 string, word2 string) int {
	len1, len2, res := len(word1), len(word2), 0
	dp := make([][]int, len1+1)
	for i := 0; i <= len1; i++ {
		dp[i] = make([]int, len2+1)
	}
	for i := 0; i < len1; i++ {
		for j := 0; j < len2; j++ {
			if word1[i] == word2[j] {
				dp[i+1][j+1] = dp[i][j] + 1
			} else {
				dp[i+1][j+1] = max(dp[i][j+1], dp[i+1][j])
			}
			res = max(res, dp[i+1][j+1])
		}
	}
	return len1 + len2 - 2*res
}
func max(a, b int) int {
	if a < b {
		return b
	}
	return a
}
```