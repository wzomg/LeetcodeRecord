# 787.K站中转内最便宜的航班
题目链接：[传送门](https://leetcode-cn.com/problems/cheapest-flights-within-k-stops/)

## 题目描述：
有n个城市通过一些航班连接。给你一个数组flights，其中flights[i] = [$from_i, to_i, price_i$] ，表示该航班都从城市 $from_i$ 开始，以价格$to_i$ 抵达 $price_i$。

现在给定所有的城市和航班，以及出发城市src和目的地dst，你的任务是找到出一条最多经过k站中转的路线，使得从src到dst的价格最便宜 ，并返回该价格。如果不存在这样的路线，则输出-1。

**提示**：

- $1 \leq n \leq 100$
- $ 0 \leq $ `flights.length` $ \leq \frac{n \times (n - 1)}{2}$
- `flights[i].length == 3`
- $0 \leq from_i, to_i < n$
- from_i != to_i
- $ 1 \leq price_i \leq 10^4$
- 航班没有重复，且不存在自环
- $ 0 \leq $ src, dst, k < n
- src != dst

**示例**：
- 输入：`n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1`
- 输出: 200
- 解释: 从城市 0 到城市 2 在 1 站中转以内的最便宜价格是 200。

## 解决方案：
- 时间复杂度：$O(w \times k)$, w表示边的总数，最多为 $ \frac{n \times (n-1)}{2} $
- 空间复杂度：$O(k \times n)$
- 思路：预处理邻接表+记忆化搜索，详解看代码注释。

## AC代码：
```go
type Link struct {
	to, cost []int
}
const MAX_INF int = 1000001
func findCheapestPrice(n int, flights [][]int, src int, dst int, k int) int {
	links := make([]Link, n)
	for i := 0; i < n; i++ {
		links[i].to = make([]int, 0)
		links[i].cost = make([]int, 0)
	}
	for _, v := range flights {
		links[v[0]].to = append(links[v[0]].to, v[1])
		links[v[0]].cost = append(links[v[0]].cost, v[2])
	}
	memo := make([][]int, n)
	for i := 0; i < n; i++ {
		memo[i] = make([]int, k+2)
	}
	ans := dfs(links, src, k+1, dst, memo) //传入 k + 1，表示最多经过的边数
	if ans == MAX_INF {
		return -1
	}
	return ans
}
//从 src 开始遍历到达 dst，且中转站个数不超过k时所花费的最小值
//memo[cur][k] 记录的是子问题的最小值，后面可能会多次经过cur节点，且剩余中转站个数为k的情况
func dfs(links []Link, cur, k, dst int, memo [][]int) int {
	if k < 0 {
		return MAX_INF
	}
	if cur == dst {
		return 0
	}
	if memo[cur][k] != 0 {
		return memo[cur][k]
	}
	res := MAX_INF
	for idx, v := range links[cur].to {
		res = min(res, dfs(links, v, k-1, dst, memo)+links[cur].cost[idx])
	}
	memo[cur][k] = res
	return res
}
func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```