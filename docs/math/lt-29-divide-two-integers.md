# 29.两数相除
题目链接：[传送门](https://leetcode-cn.com/problems/divide-two-integers/)

## 题目描述：
给定两个整数，被除数`dividend`和除数`divisor`。将两数相除，要求不使用乘法、除法和`mod`运算符。

返回被除数`dividend`除以除数`divisor`得到的商。

整数除法的结果应当截去（truncate）其小数部分，例如：truncate(8.345) = 8 以及`truncate(-2.7335) = -2`

**提示**：
- 被除数和除数均为 32 位有符号整数。
- 除数不为 0。
- 假设我们的环境只能存储 32 位有符号整数，其数值范围是 $ [−2^{31},  2^{31} − 1]$。本题中，如果除法结果溢出，则返回 $2^{31} − 1$。

**示例**:
- 输入: `dividend = 10, divisor = 3`
- 输出: 3
- 解释: `10/3 = truncate(3.33333..) = truncate(3) = 3`

## 解决方案：
- 时间复杂度：$O(log^2 n)$
- 空间复杂度：$O(1)$
- 思路：二分+快速乘法。

## AC代码：
```go
func divide(dividend int, divisor int) int {
	flag := false
	if dividend^divisor < 0 { //异或判断是否为负数
		flag = true
	}
	x, y := abs(dividend), abs(divisor) //取绝对值
	lt, rt := 0, x
	res := 0
	for lt <= rt {
		mid := ((rt - lt) >> 1) + lt
		if quick_mul(mid, y) <= x {
			res = mid
			lt = mid + 1
		} else {
			rt = mid - 1
		}
	}
	if flag {
		res = -res
	}
	if res < math.MinInt32 || res > math.MaxInt32 {
		return math.MaxInt32
	}
	return res
}
func quick_mul(a, b int) int {
	res := 0
	for b > 0 {
		if (b & 1) == 1 {
			res += a
		}
		a += a
		b >>= 1
	}
	return res
}
func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```