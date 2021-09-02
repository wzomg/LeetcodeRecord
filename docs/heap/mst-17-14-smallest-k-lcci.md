# 面试题17.14.最小K个数
题目链接：[传送门](https://leetcode-cn.com/problems/smallest-k-lcci/)

## 题目描述：
设计一个算法，找出数组中最小的k个数。以任意顺序返回这k个数均可。

**提示**：
- $ 0 \leq$  `len(arr)` $\leq 100000$
- $ 0 \leq  k \leq $ `min(100000, len(arr))`

## 解决方案：
- 时间复杂度：$O(n \times log^k)$
- 空间复杂度：$O(k)$
- 思路：建立一个只有k个元素的大顶堆即可。

## AC代码：
```go
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
func smallestK(brr []int, k int) []int {
	qMax := &hpMax{brr[:k]}
	if k < 1 { //注意判空
		return qMax.arr
	}
	//初始化大顶堆
	heap.Init(qMax)
	for _, v := range brr[k:] {
		if qMax.arr[0] > v {
			//先替换掉堆顶元素
			qMax.arr[0] = v
			//然后从堆顶往下调整为大顶堆
			heap.Fix(qMax, 0)
		}
	}
	return qMax.arr
}
```