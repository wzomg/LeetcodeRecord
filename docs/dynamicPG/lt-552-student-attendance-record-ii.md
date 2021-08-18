# 552.学生出勤记录II
题目链接：[传送门](https://leetcode-cn.com/problems/student-attendance-record-ii/)

## 题目描述：
可以用字符串表示一个学生的出勤记录，其中的每个字符用来标记当天的出勤情况（缺勤、迟到、到场）。记录中只含下面三种字符：
- 'A'：Absent，缺勤
- 'L'：Late，迟到
-'P'：Present，到场
如果学生能够 同时 满足下面两个条件，则可以获得出勤奖励：

按**总出勤**计，学生缺勤（'A'）严格 少于两天。

学生**不会**存在**连续**3天或**连续**3天以上的迟到（'L'）记录。

给你一个整数n，表示出勤记录的长度（次数）。请你返回记录长度为n时，可能获得出勤奖励的记录情况数量。答案可能很大，所以返回对 $10^9 + 7 $ 取余的结果。

**提示**：$ 1 \leq n \leq 10^5$


**示例1**：
- 输入：n = 2
- 输出：8
- 解释：有8种长度为2的记录将被视为可奖励："PP" , "AP", "PA", "LP", "PL", "AL", "LA", "LL" 只有"AA"不会被视为可奖励，因为缺勤次数为2次（需要少于2次）。

**示例2**：
- 输入：n = 10101
- 输出：183236316

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：dfs记忆化搜索，条件比较好构造，case要想清楚，详解看代码注释。

## AC代码：
```go
const MOD int = 1000000007
func checkRecord(n int) int {
	dp := make([][][]int, n)
	for i := 0; i < n; i++ {
		dp[i] = make([][]int, 2)
		for j := 0; j < 2; j++ {
			dp[i][j] = make([]int, 3)
		}
	}
	return dfs(0, n, 0, 0, dp)
}
func dfs(cur, n, absent, late int, dp [][][]int) int {
	if cur == n {
		return 1
	}
	if dp[cur][absent][late] > 0 { //记忆化
		return dp[cur][absent][late]
	}
	res := dfs(cur+1, n, absent, 0, dp) // present 随便加，注意此时要将late置为0，因为late统计的是不能连续出现3次，最多2次
	if absent < 1 {                     // absent 只能添加一次
		res = (res + dfs(cur+1, n, 1, 0, dp)) % MOD //放 absent，那么 late 就为0
	}
	if late < 2 {
		res = (res + dfs(cur+1, n, absent, late+1, dp)) % MOD // 放late，那么不用改absent，因为求的是总和
	}
	dp[cur][absent][late] = res
	return res
}
```