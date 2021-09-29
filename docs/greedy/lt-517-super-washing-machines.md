# 517.超级洗衣机
题目链接：[传送门](https://leetcode-cn.com/problems/super-washing-machines/)

## 题目描述：
假设有n台超级洗衣机放在同一排上。开始的时候，每台洗衣机内可能有一定量的衣服，也可能是空的。

在每一步操作中，你可以选择任意 m ($ 1 \leq m \leq n$) 台洗衣机，与此同时将每台洗衣机的一件衣服送到相邻的一台洗衣机。

给定一个整数数组 machines 代表从左至右每台洗衣机中的衣物数量，请给出能让所有洗衣机中剩下的衣物的数量相等的**最少的操作步数**。如果不能使每台洗衣机中衣物的数量相等，则返回-1。

**提示**：
- `n == machines.length`
- $ 1 \leq n \leq 10^4$
- $ 0 \leq$ `machines[i]` $ \leq 10^5$

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：贪心。首先有个trick：判断能否取到平均数。接下来的解法举个例子：有三个洗衣机，装的衣服数为`[0, 3, 0]`，最终状态会变为`[1, 1, 1]`，将两者做差得：`[-1, 2, -1]`。其中，负数表示当前洗衣机需要移入的衣服数，正数表示当前洗衣机需要移除的衣服数。从左往右贪心遍历，用一个前缀和变量 preSum 来记录差值数组的前缀和，若 preSum >= 0，则说明左半部分需要移出衣服，否则说明左半部分需要移入衣服，且 preSum 的值都可以交给下一个相邻的去处理。因此，移动的最大次数（最少操作步数）为差值数组和前缀和数组中出现绝对值最大的数字。

## AC代码：
```go
func findMinMoves(machines []int) int {
	sum, lenM := 0, len(machines)
	for _, v := range machines {
		sum += v
	}
	if sum%lenM != 0 {
		return -1
	}
	res, preSum, avg := 0, 0, sum/lenM
	for _, val := range machines {
		preSum += val - avg
    //组内的某一台洗衣机内的衣服数量过多，需要向两侧移出
    //左半部分移入或移出衣服数的最大值
		res = max(res, max(val-avg, abs(preSum)))
	}
	return res
}
func abs(a int) int {
	if a < 0 {
		return -a
	}
	return a
}
func max(a, b int) int {
	if a < b {
		return b
	}
	return a
}
```