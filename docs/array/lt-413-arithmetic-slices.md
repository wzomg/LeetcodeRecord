# 413.等差数列划分
题目链接：[传送门](https://leetcode-cn.com/problems/arithmetic-slices/)

## 题目描述：
如果一个数列**至少有三个元素**，并且任意两个相邻元素之差相同，则称该数列为等差数列。

例如，`[1,3,5,7,9]`、`[7,7,7,7]`和`[3,-1,-5,-9]`都是等差数列。

给你一个整数数组`nums`，返回数组`nums`中所有为等差数组的**子数组**个数。**子数组**是数组中的一个连续序列。

**提示**：

- $1 \leq $ `nums.length` $ \leq 5000$
- $1000 \leq$ `nums[i]` $\leq 1000$

**示例**：

- 输入：`nums = [1,2,3,4]`
- 输出：`3`
- 解释：`nums`中有三个子等差数组：`[1, 2, 3]`、`[2, 3, 4]`和`[1,2,3,4]`自身。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：求的是一个至少含3个元素的连续序列，从第3个元素起开始算贡献，模拟一下样例可知这些贡献累加即为当前满足条件的连续子数组的组合数，边遍历边累加组合数即可。

## AC代码：
```go
func numberOfArithmeticSlices(nums []int) int {
	arrLen := len(nums)
	if arrLen < 3 {
		return 0
	}
	d, cnt, res := nums[1]-nums[0], 0, 0
	for i := 2; i < arrLen; i++ {
		if nums[i]-nums[i-1] == d {
			cnt++
		} else {
			d = nums[i] - nums[i-1]
			cnt = 0
		}
		res += cnt
	}
	return res
}
```



