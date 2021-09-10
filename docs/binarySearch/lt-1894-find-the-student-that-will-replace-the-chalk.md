# 1894.找到需要补充粉笔的学生编号
题目链接：[传送门](https://leetcode-cn.com/problems/find-the-student-that-will-replace-the-chalk/)

## 题目描述：
一个班级里有n个学生，编号为0到n - 1。每个学生会依次回答问题，编号为0的学生先回答，然后是编号为1的学生，以此类推，直到编号为n - 1的学生，然后老师会重复这个过程，重新从编号为0的学生开始回答问题。

给你一个长度为n且下标从0开始的整数数组chalk和一个整数k。一开始粉笔盒里总共有k支粉笔。当编号为i的学生回答问题时，他会消耗`chalk[i]`支粉笔。如果剩余粉笔数量 严格小于`chalk[i]`，那么学生i需要补充粉笔。

请你返回需要补充粉笔的学生编号。

**提示**：
- `chalk.length == n`
- $ 1 \leq n \leq 10^5$
- $1 \leq$ `chalk[i]` $\leq 10^5$
- $1 \leq k \leq 10^9$

**示例**：
- 输入：`chalk = [5,1,5], k = 22`
- 输出：0
- 解释：学生消耗粉笔情况如下：

```
编号为 0 的学生使用 5 支粉笔，然后 k = 17 。
编号为 1 的学生使用 1 支粉笔，然后 k = 16 。
编号为 2 的学生使用 5 支粉笔，然后 k = 11 。
编号为 0 的学生使用 5 支粉笔，然后 k = 6 。
编号为 1 的学生使用 1 支粉笔，然后 k = 5 。
编号为 2 的学生使用 5 支粉笔，然后 k = 0 。
编号为 0 的学生没有足够的粉笔，所以他需要补充粉笔。
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：前缀和+二分查找。简单模拟一下即可。

## AC代码：
```go
func chalkReplacer(chalk []int, k int) int {
	lenC := len(chalk)
	preSum := make([]int, lenC)
	for i := 0; i < lenC; i++ {
		if i == 0 {
			preSum[i] = chalk[0]
		} else {
			preSum[i] = preSum[i-1] + chalk[i]
		}
		if k < preSum[i] {
			return i
		}
	}
	k %= preSum[lenC-1] //消除 m 轮
	//从 preSum 中找大于k的下标
	return sort.SearchInts(preSum, k+1) //这个函数若找不到val，则返回待插入的下标
}
```