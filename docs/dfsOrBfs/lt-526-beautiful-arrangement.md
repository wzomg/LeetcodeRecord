# 526.优美的排列
题目链接：[传送门](https://leetcode-cn.com/problems/beautiful-arrangement/)

## 题目描述：

假设有从1到N的N个整数，如果从这N个数字中成功构造出一个数组，使得数组的第i位 ($1 \leq i \leq N$) 满足如下两个条件中的一个，我们就称这个数组为一个优美的排列。条件：①第i位的数字能被i整除；②i能被第i位上的数字整除。现在给定一个整数N，请问可以构造多少个优美的排列？

说明：N是一个正整数，并且不会超过15。

**示例**:

- 输入: 2
- 输出: 2
- 解释: 

```
第 1 个优美的排列是 [1, 2]:
  第 1 个位置（i=1）上的数字是1，1能被 i（i=1）整除
  第 2 个位置（i=2）上的数字是2，2能被 i（i=2）整除
第 2 个优美的排列是 [2, 1]:
  第 1 个位置（i=1）上的数字是2，2能被 i（i=1）整除
  第 2 个位置（i=2）上的数字是1，i（i=2）能被 1 整除
```


## 解决方案：
- 时间复杂度：$O(n!)$
- 空间复杂度：$O(n)$
- 思路：简单暴搜。预处理出每个位置i符合条件的数字有哪些，然后标记哪些数字已使用，直到 $ 1 \sim n $ 所有数字都被使用为止才将结果加1。

## AC代码：
```go
func countArrangement(n int) int {
	vis := make([]bool, n+1)
	//小优化，先找出每个下标i能满足条件的那个数字 j
	match := make([][]int, n+1)
	for i := 1; i <= n; i++ {
		for j := 1; j <= n; j++ {
			if i%j == 0 || j%i == 0 {
				match[i] = append(match[i], j)
			}
		}
	}
	return dfs(vis, 1, n, match)
}
func dfs(vis []bool, idx, n int, match [][]int) int {
	if idx > n { //若 idx > n，则说明已经放完了
		return 1
	}
	res := 0
	for _, val := range match[idx] { //尝试取出第 idx 位置上的数字
		if !vis[val] { //若val还没使用
			vis[val] = true
			res += dfs(vis, idx+1, n, match)
			vis[val] = false
		}
	}
	return res
}
```