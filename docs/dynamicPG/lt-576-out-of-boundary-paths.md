# 576.出界的路径数
题目链接：[传送门](https://leetcode-cn.com/problems/out-of-boundary-paths/)

## 题目描述：

给你一个大小为$m \times n$ 的网格和一个球。球的起始坐标为`[startRow, startColumn]`。你可以将球移到在四个方向上相邻的单元格内（可以穿过网格边界到达网格之外）。你**最多**可以移动`maxMove`次球。

给你五个整数m、n、maxMove、startRow 以及 startColumn ，找出并返回可以将球移出边界的路径数量。因为答案可能非常大，返回对 $10^9 + 7 $取余后的结果。

**提示**：

- $1 <\leq m, n \leq 50$
- $0 \leq$ `maxMove` $ \leq 50$
- $0 \leq$ `startRow` $ < m$
- $0 \leq$ `startColumn` $< n$

## 解决方案：
- 时间复杂度：$O(maxMove \times m \times n)$
- 空间复杂度：$O(maxMove \times m \times n)$
- 思路：计数dp。定义 dp[i][j][k]：表示球移动 i 次之后位于坐标 $ (j, k)$ 的路径数量，那么初始位置的值为`dp[0][startRow][startColumn]=1`，对于当前坐标$(j,k)$，下一个状态转移方程为（四个方向）：$dp[i+1][j_m][k_m] += dp[i][j][k] $。

## AC代码：
```go
func findPaths(m int, n int, maxMove int, startRow int, startColumn int) (res int) {
	const mod int = 1000000007
	dirs := [][]int{{-1, 0}, {1, 0}, {0, 1}, {0, -1}}
	dp := make([][][]int, maxMove+1)
	for i := 0; i <= maxMove; i++ {
		dp[i] = make([][]int, m)
		for j := 0; j < m; j++ {
			dp[i][j] = make([]int, n)
		}
	}
	dp[0][startRow][startColumn] = 1
	for i := 0; i < maxMove; i++ {
		for j := 0; j < m; j++ {
			for k := 0; k < n; k++ {
				preNum := dp[i][j][k]
				if preNum < 1 {
					continue
				}
				for z := 0; z < 4; z++ {
					dx, dy := dirs[z][0]+j, dirs[z][1]+k
					if check(dx, dy, m, n) {
						dp[i+1][dx][dy] = (dp[i+1][dx][dy] + preNum) % mod
					} else {
						res = (res + preNum) % mod
					}
				}
			}
		}
	}
	return
}
func check(x, y, m, n int) bool {
	return x >= 0 && y >= 0 && x < m && y < n
}
```