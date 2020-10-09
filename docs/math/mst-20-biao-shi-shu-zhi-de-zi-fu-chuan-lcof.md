# 面试题20.表示数值的字符串
题目链接：[传送门](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

## 题目描述：
请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串`"+100"`、`"5e2"`、`"-123"`、`"3.1416"`、`"-1E-16"`、`"0123"`都表示数值，但`"12e"`、`"1a3.14"`、`"1.2.3"`、`"+-5"`及`"12e+5.4"`都不是。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：模拟题。若字符串中有`'e'`或者`E`，则其前半部分可能是小数或整数，并且后半部分只能是整数。

## AC代码：
```java
class Solution {
	public boolean isNumber(String s) {
		// 1、先去首尾空格
		s = s.trim();
		// 若为空，则不是数值
		if (s.length() == 0)
			return false;
		int index = s.length();
		for (int i = 0; i < s.length(); i++)
			if (s.charAt(i) == 'e' || s.charAt(i) == 'E')
				index = i;
		// 没有 e 或 E
		if (index == s.length())
			return isNum(s);
		// e 或 E 在最后一个位置或者第一个位置，肯定是错的
		if (index == s.length() - 1 || index == 0)
			return false;
		// 将 e 处为中轴，划分2部分，前一半可能为小数或整数，后一半一定是整数
		return isNum(s.substring(0, index)) && isInteger(s.substring(index + 1));
	}
	// 不包含 e 和 E，判断s是否为数值，只要小数点个数不超过1个并且出现数字，除了开头只能是 '+' 或者 '-'
	private boolean isNum(String s) {
		int i = 0;
		boolean flag = false;
		if (s.charAt(0) == '+' || s.charAt(0) == '-')
			i++;
		// 统计小数点的个数，除了开头有 '+' 或者 '-'，其余只能是数字或者小数点
		int pointCnt = 0;
		for (int len = s.length(); i < len; i++) {
			if (s.charAt(i) == '.')
				pointCnt++;
			else if (s.charAt(i) < '0' || s.charAt(i) > '9')
				return false;
			else
				flag = true;
			// 超过1个小数点
			if (pointCnt > 1)
				return false;
		}
		return flag;
	}
	// 判断是否为纯整数，必须有数字，除了开头只能是 '+' 或者 '-'
	private boolean isInteger(String s) {
		int i = 0;
		if (s.charAt(0) == '+' || s.charAt(0) == '-')
			i++;
		boolean flag = false;
		for (int len = s.length(); i < len; i++) {
			if (s.charAt(i) < '0' || s.charAt(i) > '9')
				return false;
			else
				flag = true;
		}
		return flag;
	}
}
```