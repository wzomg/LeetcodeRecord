# 8.字符串转换整数(atoi)
题目链接：[传送门](https://leetcode-cn.com/problems/string-to-integer-atoi/)

## 题目描述：
请你来实现一个`myAtoi(string s) `函数，使其能将字符串转换成一个`32`位有符号整数（类似 C/C++ 中的`atoi`函数）。

函数`myAtoi(string s)`的算法如下：

- 读入字符串并丢弃无用的前导空格
- 检查下一个字符（假设还未到字符末尾）为正还是负号，读取该字符（如果有）。 确定最终结果是负数还是正数。如果两者都不存在，则假定结果为正。
- 读入下一个字符，直到到达下一个非数字字符或到达输入的结尾。字符串的其余部分将被忽略。
- 将前面步骤读入的这些数字转换为整数（即，`"123" -> 123`，`"0032" -> 32`）。如果没有读入数字，则整数为`0`。必要时更改符号（从步骤 2开始）。
- 如果整数数超过 32 位有符号整数范围 $ [−2^{31},  2^{31} − 1]$ ，需要截断这个整数，使其保持在这个范围内。具体来说，小于 $−2^{31}$ 的整数应该被固定为 $−2^{31}$ ，大于 $2^{31} − 1$ 的整数应该被固定为 $2^{31} − 1$。
返回整数作为最终结果。

**注意**：

- 本题中的空白字符只包括空格字符`' '`。
- 除前导空格或数字后的其余字符串外，请勿忽略 任何其他字符。

**提示**：

- $ 0 \le s.length \le 200$
- s 由英文字母（大写和小写）、数字（$0 \sim 9$）、`' '`、`'+'`、`'-'` 和 `'.'` 组成。

**示例1**：

- 输入：`s = "42"`
- 输出：42
- 解释：加粗的字符串为已经读入的字符，插入符号是当前读取的字符。
```
第 1 步："42"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："42"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："42"（读入 "42"）
           ^
解析得到整数 42 。
由于 "42" 在范围 [-231, 231 - 1] 内，最终结果为 42 。
```

**示例2**：
- 输入：`s = "words and 987"`
- 输出：0
- 解释：
```
第 1 步："words and 987"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："words and 987"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："words and 987"（由于当前字符 'w' 不是一个数字，所以读入停止）
         ^
解析得到整数 0 ，因为没有读入任何数字。
由于 0 在范围 [-231, 231 - 1] 内，最终结果为 0 。
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：简单模拟即可。详解看注释！

## AC代码：
```java
class Solution {
	public int myAtoi(String s) {
		int len;
		if (s == null || (len = s.length()) == 0)
			return 0;
		int res = 0;
		boolean flag = false; // 标记第一个字符
		boolean op = true; // 默认为正整数
		int fuhaoNum = 0;
		for (int i = 0; i < len; i++) {
			// 还没出现 +、-或数字字符
			if (!flag) {
				if ('0' <= s.charAt(i) && s.charAt(i) <= '9') {
					// 需要判断一下上一个字符是否为减号
					if (i > 0 && s.charAt(i - 1) == '-')
						op = false; // op=false表示为负数
					res = s.charAt(i) - '0';
					flag = true;
				} else if (s.charAt(i) == '-' || s.charAt(i) == '+') {
					fuhaoNum++;
					// 若符号为1个，并且下一个字符不是数字，则直接返回0
					if (fuhaoNum > 1 || (i + 1 < len && !('0' <= s.charAt(i + 1) && s.charAt(i + 1) <= '9')))
						return 0;
				} else if (s.charAt(i) != ' ') // 剩下的前导字符不是空格，则直接返回0
					return 0;
			} else {
				// 当前是一个连续的字符
				if ('0' <= s.charAt(i) && s.charAt(i) <= '9') {
					if (op && (res > Integer.MAX_VALUE / 10
							|| (res == Integer.MAX_VALUE / 10 && (s.charAt(i) - '0') > 7)))
						return Integer.MAX_VALUE;
					if (!op && (res > Integer.MAX_VALUE / 10
							|| (res == Integer.MAX_VALUE / 10 && (s.charAt(i) - '0') > 8)))
						return Integer.MIN_VALUE;
					res = res * 10 + s.charAt(i) - '0';
				} else
					break;

			}
		}
		return !op ? res * -1 : res;
	}
}
```