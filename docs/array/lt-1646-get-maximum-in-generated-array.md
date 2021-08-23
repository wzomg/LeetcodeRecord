# 1646.获取生成数组中的最大值
题目链接：[传送门](https://leetcode-cn.com/problems/get-maximum-in-generated-array/)

## 题目描述：
给你一个整数n。按下述规则生成一个长度为n + 1的数组nums：
- nums[0] = 0
- nums[1] = 1
- 当 $ 2 \leq 2 \times i \leq n$ 时，nums[$2 \times i$] = nums[i]
- 当 $ 2 \leq 2 \times i + 1 \leq n$ 时，nums[$2 \times i + 1$] = nums[i] + nums[i + 1]
- 返回生成数组 nums 中的 最大 值。

**提示**：$0 \leq n \leq 100$
- 输入：n = 7
- 输出：3
- 解释：根据规则：

```
nums[0] = 0
nums[1] = 1
nums[(1 * 2) = 2] = nums[1] = 1
nums[(1 * 2) + 1 = 3] = nums[1] + nums[2] = 1 + 1 = 2
nums[(2 * 2) = 4] = nums[2] = 1
nums[(2 * 2) + 1 = 5] = nums[2] + nums[3] = 1 + 2 = 3
nums[(3 * 2) = 6] = nums[3] = 2
nums[(3 * 2) + 1 = 7] = nums[3] + nums[4] = 2 + 1 = 3
因此，nums = [0,1,1,2,1,3,2,3]，最大值 3
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：简单模拟即可。

## AC代码：
```go
func getMaximumGenerated(n int) int {
	if n < 1 {
		return 0
	}
	res := 1
	nums := make([]int, n+1)
	nums[1] = 1
	for i := 2; i <= n; i++ {
		if i%2 == 0 {
			nums[i] = nums[i/2]
		} else {
			nums[i] = nums[(i-1)/2] + nums[(i-1)/2+1]
		}
		res = max(res, nums[i])
	}
	return res
}
func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```