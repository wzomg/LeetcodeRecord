# 852.山脉数组的峰顶索引
题目链接：[传送门](https://leetcode-cn.com/problems/peak-index-in-a-mountain-array/)

## 题目描述：
符合下列属性的数组`arr`称为**山脉数组**：
- `arr.length >= 3`
- 存在 i（0 < i < arr.length - 1）使得：
    - `arr[0] < arr[1] < ... arr[i-1] < arr[i]`
    - `arr[i] > arr[i+1] > ... > arr[arr.length - 1]`

给你由整数组成的山脉数组arr，返回任何满足`arr[0] < arr[1] < ... arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`的下标 i 。

**提示**：
- $ 3 \leq$ `arr.length` $ \leq 10^4$
- $0 \leq$ `arr[i]` $ \leq 10^6$
- 题目数据保证`arr`是一个山脉数组

**进阶**：很容易想到时间复杂度 $O(n)$ 的解决方案，你可以设计一个 $O(log^n)$ 的解决方案吗？

**示例**：
- 输入：`arr = [0,2,1,0]`
- 输出：1

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(1)$
- 思路：简单二分搜峰值。

## AC代码：
```go
func peakIndexInMountainArray(arr []int) int {
	lt, rt := 0, len(arr)-1
	for lt < rt {
		mid := ((rt - lt) >> 1) + lt
		if arr[mid] >= arr[mid+1] {
			rt = mid
		} else {
			lt = mid + 1
		}
	}
	return lt
}
```