# 447.回旋镖的数量
题目链接：[传送门](https://leetcode-cn.com/problems/number-of-boomerangs/)

## 题目描述：
给定平面上n对**互不相同**的点points，其中`points[i] = [xi, yi]`。回旋镖 是由点`(i, j, k)`表示的元组 ，其中i和j之间的距离和i和k之间的距离相等（需要考虑元组的顺序）。

返回平面上所有回旋镖的数量。

**提示**：
- `n == points.length`
- $1 \leq n \leq 500$
- `points[i].length == 2`
- $-10^4 \leq x_i, y_i \leq 10^4$
- 所有点都**互不相同**

**示例**：
- 输入：`points = [[0,0],[1,0],[2,0]]`
- 输出：2
- 解释：两个回旋镖为`[[1,0],[0,0],[2,0]]`和`[[1,0],[2,0],[0,0]]`

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n)$
- 思路：枚举坐标点i，找出所有与i距离相同的点，将它们的曼哈顿距离的平方放到一个哈希表中，因为需要考虑元组顺序，假设到坐标点i距离相同的点个数为m，则排列数为$A_m^2 = (m - 1) \times m$，累加所有排列数即可。

## AC代码：
```go
func numberOfBoomerangs(points [][]int) int {
	res := 0
	for _, q := range points {
		cnt := map[int]int{}
		for _, p := range points {
			cnt[(q[0]-p[0])*(q[0]-p[0])+(q[1]-p[1])*(q[1]-p[1])]++
		}
		for _, v := range cnt {
			res += v * (v - 1)
		}
	}
	return res
}
```