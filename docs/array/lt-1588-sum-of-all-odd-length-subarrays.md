# 1588.所有奇数长度子数组的和
题目链接：[传送门](https://leetcode-cn.com/problems/sum-of-all-odd-length-subarrays/)

## 题目描述：
给你一个正整数数组arr，请你计算所有可能的奇数长度子数组的和。

子数组定义为原数组中的一个连续子序列。

请你返回arr中所有奇数长度子数组的和 。

**提示**：
- $1 \leq$  `arr.length` $ \leq 100$
- $1 \leq$ `arr[i]` $\leq 1000$


**示例**：
- 输入：`arr = [1,4,2,5,3]`
- 输出：58
- 解释：所有奇数长度子数组和它们的和为：

```
[1] = 1
[4] = 4
[2] = 2
[5] = 5
[3] = 3
[1,4,2] = 7
[4,2,5] = 11
[2,5,3] = 10
[1,4,2,5,3] = 15
我们将所有值求和得到 1 + 4 + 2 + 5 + 3 + 7 + 11 + 10 + 15 = 58
```

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n)$
- 思路：简单前缀和。

## AC代码：
```go
func sumOddLengthSubarrays(arr []int) int {
	tot, lenA := 0, len(arr)
	if lenA < 1 {
		return tot
	}
	preSum := make([]int, lenA)
	preSum[0] = arr[0]
	for i := 1; i < lenA; i++ {
		preSum[i] = arr[i] + preSum[i-1]
	}
	for step := 1; step <= lenA; step += 2 {
		for i := 0; i+step-1 < lenA; i++ {
			tot += preSum[i+step-1]
			if i > 0 {
				tot -= preSum[i-1]
			}
		}
	}
	return tot
}
```