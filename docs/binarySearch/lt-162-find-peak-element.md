# 162.寻找峰值
题目链接：[传送门](https://leetcode-cn.com/problems/find-peak-element/)

## 题目描述：
峰值元素是指其值严格大于左右相邻值的元素。

给你一个整数数组`nums`，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回**任何一个峰值**所在位置即可。

你可以假设`nums[-1] = nums[n] = -∞`。

你必须实现时间复杂度为$O(log^n)$的算法来解决此问题。

**提示**：
- $ 1 \leq$ `nums.length` $ \leq 1000$
- $-2^{31} \leq$ `nums[i]` $ \leq 2^{31} - 1$
- 对于所有有效的i都有`nums[i] != nums[i + 1]`

**示例**：
- 输入：`nums = [1,2,3,1]`
- 输出：2
- 解释：3是峰值元素，你的函数应该返回其索引2。

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(1)$
- 思路：二分查找。围绕上下坡条件来判断即可。

## AC代码：
```go
func findPeakElement(nums []int) int {
	lt, rt, mid := 0, len(nums)-1, 0
	for lt < rt { //lt < rt 条件可以保证 mid + 1 不超过 len - 1
		mid = ((rt - lt) >> 1) + lt
		if nums[mid] > nums[mid+1] { //下坡，那mid可能是坡顶，左移右边界
			rt = mid
		} else { //上坡，那mid+1可能是坡顶，右移左边界
			lt = mid + 1
		}
	}
	return lt
}
```