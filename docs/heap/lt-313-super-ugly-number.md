# 313.超级丑数
题目链接：[传送门](https://leetcode-cn.com/problems/super-ugly-number/)

## 题目描述：
**超级丑数**是一个正整数，并满足其所有质因数都出现在质数数组`primes`中。

给你一个整数`n`和一个整数数组`primes`，返回第`n`个**超级丑数**。

题目数据保证第`n`个**超级丑数**在`32-bit`带符号整数范围内。

## 解决方案：
- 时间复杂度：$O(n \times m \times log^{n \times m})$
- 空间复杂度：$O(n \times m)$
- 思路：维护一个小顶堆，每次弹出堆顶最小元素，将其与prime数组中每个数相乘，注意去重，一共弹出n次即可。

**提示**：

- $1 \leq n \leq 10^6$
- $1 \leq $ `primes.length` $ \leq 100$
- $2 \leq$ `primes[i]` $\leq 1000$
- 题目数据保证`primes[i]`是一个质数
- `primes` 中的所有值都**互不相同**，且按**递增顺序**排列

**示例**：

- 输入：`n = 12`, `primes = [2,7,13,19]`
- 输出：`32` 
- 解释：给定长度为4的质数数组`primes = [2,7,13,19]`，前12个超级丑数序列为：`[1,2,4,7,8,13,14,16,19,26,28,32]`。

## AC代码：
```go
type hp struct {
	sort.IntSlice
}

//实现 sort.Interface 接口，即重写两个方法
func (h *hp) Push(v interface{}) {
	h.IntSlice = append(h.IntSlice, v.(int))
}

func (h *hp) Pop() interface{} {
	old := h.IntSlice
	lastVal := old[len(old)-1]
	h.IntSlice = old[:len(old)-1]
	return lastVal
}

func nthSuperUglyNumber(n int, primes []int) (ugly int) {
	visMap := map[int]bool{1: true} //标记是否出现过
	ph := &hp{[]int{1}}
	for ; n > 0; n-- {
		ugly = heap.Pop(ph).(int) //堆顶元素，数组最后一个元素
		for _, val := range primes {
			if nextVal := val * ugly; !visMap[nextVal] { //没有出现过才添加到小顶堆里
				visMap[nextVal] = true
				heap.Push(ph, nextVal)
			}
		}
	}
	return
}
```