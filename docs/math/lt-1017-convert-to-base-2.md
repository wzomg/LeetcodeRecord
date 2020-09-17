# 1017.负二进制转换
题目链接：[传送门](https://leetcode-cn.com/problems/convert-to-base-2/)

## 题目描述：
给出数字`N`，返回由若干`"0"`和`"1"`组成的字符串，该字符串为`N`的负二进制（base -2）表示。

除非字符串就是`"0"`，否则返回的字符串中不能含有前导零。

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(1)$
- 思路：辗转相除法。正二进制计算时是向下取整，负二进制是向上取整。

## AC代码：
```java
class Solution {
	public String baseNeg2(int N) {
		if (N == 0)
			return "0";
		StringBuilder sb = new StringBuilder();
		int res;
		while (N != 0) {
			// 负数取余负数，若结果不为0，则余数为负数
			res = N % -2;
			// 取余2只有3种结果，-1，0，1，-1时就取为1
			sb.append(res == -1 ? 1 : res);
			N /= -2;
			// 只有当余数为-1时，相当于向上取整
			// 比如 5 / -2 == -2（-2.5实际向上取整为-2），此时余数不为-1：5 % -2 == 1，即不用加1
			if (res == -1)
				N++;
		}
		return sb.reverse().toString();
	}
}
```