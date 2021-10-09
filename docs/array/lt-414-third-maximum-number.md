# 414.第三大的数
题目链接：[传送门](https://leetcode-cn.com/problems/third-maximum-number/)

## 题目描述：
给你一个非空数组，返回此数组中**第三大的数**。如果不存在，则返回数组中最大的数。

**提示**：
- $ 1 \leq$ `nums.length` $ \leq 10^4$
- $ -2^{31} \leq$ `nums[i]` $ \leq 2^{31} - 1$

**进阶**：你能设计一个时间复杂度 $O(n)$ 的解决方案吗？

**示例**：
- 输入：`[2, 2, 3, 1]`
- 输出：`1`
- 解释：注意，要求返回第三大的数，是指在所有不同数字中排第三大的数。此例中存在两个值为2的数，它们都排第二。在所有不同数字中排第三大的数为1。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：用3个变量简单模拟，注意去重即可。

## AC代码：
```go
func thirdMax(nums []int) int {
	first, second, thrid := math.MinInt64, math.MinInt64, math.MinInt64
	for _, val := range nums {
		if val > first {
			thrid = second
			second = first
			first = val
		} else if val == first { //过滤出现相同的
			continue
		} else if val > second {
			thrid = second
			second = val
		} else if val == second { //过滤出现相同的
			continue
		} else if val > thrid {
			thrid = val
		}
	}
	if thrid == math.MinInt64 { //不存在第三大的数字，返回最大的
		return first
	}
	return thrid
}
```