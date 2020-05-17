# 7.整数反转
题目链接：[传送门](https://leetcode-cn.com/problems/reverse-integer/)

## 题目描述：
给出一个`32`位的有符号整数，你需要将这个整数中每位上的数字进行反转。

**注意**:

假设我们的环境只能存储得下`32`位的有符号整数，则其数值范围为 $ [−2^{31},  2^{31} − 1] $。请根据这个假设，如果反转后整数溢出那么就返回`0`。

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(1)$
- 思路：简单数学。若数据没有溢出，则循环迭代表达式直到`x==0`：`y = y * 10 + x % 10; x /= 10;`；否则有4种临界情况：①$ y > 214748364 $；②$ y < -214748364 $；③$ y == 214748364 \land x \% 10 > 7$；④$ y == -214748364 \land x \% 10 < -8$，以上`y`再迭代一次都将会发生数据溢出。 

## AC代码：
```java
class Solution {
	public int reverse(int x) {
		int y = 0;
		while (x != 0) {
			if (y > Integer.MAX_VALUE / 10 || (y == Integer.MAX_VALUE / 10 && (x % 10 > 7)))
				return 0;
			if (y < Integer.MIN_VALUE / 10 || (y == Integer.MIN_VALUE / 10 && (x % 10 < -8)))
				return 0;
			y = y * 10 + (x % 10);
			x /= 10;
		}
		return y;
	}
}
```