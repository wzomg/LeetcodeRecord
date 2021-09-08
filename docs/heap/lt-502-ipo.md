# 502.IPO
题目链接：[传送门](https://leetcode-cn.com/problems/ipo/)

## 题目描述：
假设力扣（LeetCode）即将开始IPO。为了以更高的价格将股票卖给风险投资公司，力扣希望在IPO之前开展一些项目以增加其资本。由于资源有限，它只能在IPO之前完成最多k个不同的项目。帮助力扣设计完成最多k个不同项目后得到最大总资本的方式。

给你n个项目。对于每个项目i，它都有一个纯利润`profits[i]`，和启动该项目需要的最小资本`capital[i]`。

最初，你的资本为w。当你完成一个项目时，你将获得纯利润，且利润将被添加到你的总资本中。

总而言之，从给定项目中选择**最多k个**不同项目的列表，以最大化最终资本，并输出最终可获得的最多资本。

答案保证在32位有符号整数范围内。

**提示**：
- $1 \leq k \leq 10^5$
- $0 \leq w \leq 10^9$
- `n == profits.length`
- `n == capital.length`
- $ 1 \leq n \leq 10^5$
- $ 0 \leq$ `profits[i]` $ \leq 10^4$
- $0 \leq$ `capital[i]` $ \leq 10^9$

**示例**：
- 输入：`k = 2, w = 0, profits = [1,2,3], capital = [0,1,1]`
- 输出：4
- 解释：

```
由于你的初始资本为 0，你仅可以从 0 号项目开始。
在完成后，你将获得 1 的利润，你的总资本将变为 1。
此时你可以选择开始 1 号或 2 号项目。
由于你最多可以选择两个项目，所以你需要完成 2 号项目以获得最大的资本。
因此，输出最后最大化的资本，为 0 + 1 + 3 = 4。
```

## 解决方案：
- 时间复杂度：$O(n \times log^n)$
- 空间复杂度：$O(n)$
- 思路：贪心+大顶堆。注意：题目说了，只要当前资本能启动一个项目，那么该项目的利润就能添加到当前总资本中，不会扣除启动资金。因此，我们需要维护当前启动资金不大于总资本的项目，从中取出利润最大的那个，这样总资本就更大了，能容纳更大的启动资金，最后获取的总利润也最多。

## AC代码：
```go
type node struct {
	p, c int
}
func findMaximizedCapital(k int, w int, profits []int, capital []int) int {
	lenN := len(profits)
	nodes := make([]node, lenN)
	for i := 0; i < lenN; i++ {
		nodes[i] = node{profits[i], capital[i]} //将利润和启动资金绑定在一起
	}
	sort.Slice(nodes, func(i, j int) bool { return nodes[i].c < nodes[j].c }) //按照启动资金从小到大排序
	h := &hpMax{}
	for idx := 0; k > 0; k-- { //最多只能完成k个项目
		for idx < lenN && nodes[idx].c <= w { //若满足启动资金不大于现有资本，则添加该利润到大顶堆里
			heap.Push(h, nodes[idx].p)
			idx++
		}
		if h.Len() > 0 {
			w += heap.Pop(h).(int) //累加当前资金能启动的项目获得的利润
		}
	}
	return w
}
type hpMax struct {
	arr []int
}
//实现 sort.Interface 接口，即重写两个方法
func (h *hpMax) Push(v interface{}) {
	h.arr = append(h.arr, v.(int))
}
func (h *hpMax) Pop() interface{} {
	old := h.arr
	lastVal := old[len(old)-1]
	h.arr = old[:len(old)-1]
	return lastVal
}
// 这里决定大、小顶堆 现在是大顶堆
func (h hpMax) Less(i, j int) bool { return h.arr[i] > h.arr[j] }
func (h hpMax) Swap(i, j int) {
	h.arr[i], h.arr[j] = h.arr[j], h.arr[i]
}
func (h hpMax) Len() int { return len(h.arr) }
```