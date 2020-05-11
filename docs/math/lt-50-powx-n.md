# 50.Pow(x, n)
题目链接：[传送门](https://leetcode-cn.com/problems/powx-n/)

## 题目描述：
实现`pow(x, n)`，即计算`x`的`n`次幂函数。

**说明**:

- $ -100.0 < x < 100.0 $
- `n`是`32`位有符号整数，其数值范围是$ [−2^{31}, 2^{31} − 1]$。

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(1)$
- 思路：整数快速幂。思路就是将指数`n`拆成二进制`m`，其中有`1`所对应的10进制数为`b`，累乘 $ x^b $ 即可。注意：以下算法中`n`为非负数。

## AC代码：
```java
class Solution {
	public double myPow(double x, int n) {
		double res = 1.0;
		boolean flag = false;
		long b = n;
		if (b < 0) {
			flag = true;
			b = -b; // 将负数转化为正数
		}
		while (b > 0) {
			if ((b & 1) == 1)
				res = res * x;
			x *= x;
			b >>= 1;
		}
		return flag ? 1.0 / res : res;
	}
}
```