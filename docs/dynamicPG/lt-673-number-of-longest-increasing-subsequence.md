# 673.最长递增子序列的个数
题目链接：[传送门](https://leetcode-cn.com/problems/number-of-longest-increasing-subsequence/)

## 题目描述：
给定一个未排序的整数数组，找到最长递增子序列的个数。

注意: 给定的数组长度不超过 2000 并且结果一定是32位有符号整数。

**示例**:
- 输入: `[1,3,5,4,7]`
- 输出: 2
- 解释: 有两个最长递增子序列，分别是`[1, 3, 4, 7]`和`[1, 3, 5, 7]`。

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n)$
- 思路：动态规划。定义：`dp[i]`表示以`nums[i]`结尾的最长上升子序列的长度，`cnt[i]`表示以`nums[i]`结尾的最长上升子序列的个数。状态转移方程看代码及注释。

## AC代码：
```go
func findNumberOfLIS(nums []int) int {
	lenN, maxL, res := len(nums), 0, 0
	dp, cnt := make([]int, lenN), make([]int, lenN)
	for i, x := range nums {
		dp[i] = 1
		cnt[i] = 1
		for j, y := range nums[:i] {
			if x > y {
				if dp[j]+1 > dp[i] { // 更新最长长度的上升子序列
					dp[i] = dp[j] + 1
					cnt[i] = cnt[j] // 重置计数
				} else if dp[j]+1 == dp[i] { // 说明遇到相同长度的最长上升子序列，则累加相同长度的子序列总数
					cnt[i] += cnt[j]
				}
			}
		}
		if dp[i] > maxL {
			maxL = dp[i]
			res = cnt[i] //重置计数
		} else if dp[i] == maxL { //若出现相同长度，则累加相同长度的子序列总数
			res += cnt[i]
		}
	}
	return res
}
```