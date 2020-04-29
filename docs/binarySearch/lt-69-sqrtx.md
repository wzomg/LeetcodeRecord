# 69. x 的平方根
题目链接：[传送门](https://leetcode-cn.com/problems/sqrtx/)

## 题目描述：
实现`int sqrt(int x)`函数。

计算并返回`x`的平方根，其中`x`是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

**示例**:

- 输入: 4
- 输出: 2

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(1)$
- 思路：二分查找。注意要用`long`避免乘法溢出。

## AC代码：
```java
class Solution {
	public int mySqrt(int x) {
		int lt = 0, rt = x, res = 0, mid;
		while (lt <= rt) {
			mid = (lt + rt) >> 1;
			if ((long) mid * mid > x) // 注意乘法溢出
				rt = mid - 1;
			else { // mid * mid <= x，靠右找
				lt = mid + 1;
				res = mid; // 记录值
			}
		}
		return res;
	}
}
```
