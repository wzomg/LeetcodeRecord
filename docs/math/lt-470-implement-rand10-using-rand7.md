# 470.用Rand7()实现Rand10()
题目链接：[传送门](https://leetcode-cn.com/problems/implement-rand10-using-rand7/)

## 题目描述：
已有方法rand7可生成1到7范围内的均匀随机整数，试写一个方法rand10生成1到10范围内的均匀随机整数。

不要使用系统的Math.random()方法。

**提示**:
- rand7 已定义。
- 传入参数: n 表示 rand10 的调用次数。

**进阶**:
- rand7()调用次数的期望值是多少?
- 你能否尽量少调用rand7()?

**示例**:
- 输入: 1
- 输出: [7]

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：拒绝采样，基于这样一个事实`(randX() - 1) * Y + randY()`可以等概率的生成`[1, X * Y]`范围的随机数

## AC代码：
```go
func rand10() int {
	for {
		x, y := rand7(), rand7()
		num := (x-1)*7 + y
		if num <= 40 {
			return num%10 + 1
		}
	}
}
```