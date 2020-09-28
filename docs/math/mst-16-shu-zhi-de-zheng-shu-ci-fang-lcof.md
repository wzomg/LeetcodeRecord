# 面试题16.数值的整数次方
题目链接：[传送门](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

## 题目描述：
实现函数`double Power(double base, int exponent)`，求`base`的`exponent`次方。不得使用库函数，同时不需要考虑大数问题。

**说明**：

- $-100.0 < x < 100.0$
- `n`是`32`位有符号整数，其数值范围是 $[−2^{31}, 2^{31} − 1]$。

**示例**:

- 输入：`2.00000`, `-2`
- 输出：`0.25000`
- 解释：$2^{-2} = \frac{1}{22} = \frac{1}{4} = 0.25 $

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(1)$
- 思路：快速幂。注意数据溢出！

## AC代码：
```java
class Solution {
	public double myPow(double x, int n) {
		double res = 1;
		// 先将n存放到b中，避免-2147483648转成正数时发生数据溢出
		long b = n;
		if (b < 0)
			b = -b;
		while (b > 0) {
			if ((b & 1) == 1)
				res = res * x;
			x *= x;
			b >>= 1;
		}
		return n < 0 ? 1.0 / res : res;
	}
}
```