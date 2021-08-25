# 797.所有可能的路径
题目链接：[传送门](https://leetcode-cn.com/problems/all-paths-from-source-to-target/)

## 题目描述：
给你一个有n个节点的 有向无环图（DAG），请你找出所有从节点0到节点n-1的路径并输出（不要求按特定顺序）

二维数组的第i个数组中的单元都表示有向图中i号节点所能到达的下一些节点，空就是没有下一个结点了。

译者注：有向图是有方向的，即规定了a→b你就不能从b→a。

**提示**：
- `n == graph.length`
- $ 2 \leq n \leq 15$
- $0 \leq$ `graph[i][j]` < n
- graph[i][j] != i（即不存在自环）
- graph[i] 中的所有元素**互不相同**
- 保证输入为有向无环图（DAG）

**示例**：
- 输入：`graph = [[1,2],[3],[3],[]]`
- 输出：`[[0,1,3],[0,2,3]]`
- 解释：有两条路径`0 -> 1 -> 3`和`0 -> 2 -> 3`

## 解决方案：
- 时间复杂度：$O(n \times 2^n)$
- 空间复杂度：$O(n^2)$
- 思路：简单暴搜，注意切片append后可能扩容导致地址发生变化。

## AC代码：
```go
//注意：切片在append最好声明在全局变量，不能作为形参传递，否则最终结果为空。要使用形参，则要传递二维切片的地址
func allPathsSourceTarget(graph [][]int) [][]int {
	res := make([][]int, 0) //注意：未知长度要初始为空切片，否则多次执行会累加结果
	lenG := len(graph)
	dfs(graph, []int{0}, 0, lenG, &res) //传入二维切片地址
	return res
}
func dfs(graph [][]int, list []int, cur, lenG int, res *[][]int) {
	if cur == lenG-1 {
		newList := make([]int, len(list))
		copy(newList, list) //拷贝一份新的一维切片
		*res = append(*res, newList)
		return
	}
	for _, v := range graph[cur] {
		list = append(list, v)
		dfs(graph, list, v, lenG, res)
		list = list[:len(list)-1]
	}
}
```