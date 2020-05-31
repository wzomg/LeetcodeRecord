# 394.字符串解码
题目链接：[传送门](https://leetcode-cn.com/problems/decode-string/)

## 题目描述：
给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为：`k[encoded_string]`，表示其中方括号内部的`encoded_string`正好重复`k`次。注意`k`保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数`k`，例如不会出现像`3a`或`2[4]` 的输入。

**示例**:

- s = `"3[a]2[bc]"`, 返回 `"aaabcbc"`.
- s = `"3[a2[c]]"`, 返回 `"accaccacc"`.
- s = `"2[abc]3[cd]ef"`, 返回 `"abcabccdcdcdef"`.

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：暴搜，每遇到左括号就递归，遇到右括号就返回当前位置和子串，详解看代码注释。

## AC代码：
```java
class Solution {
	public String decodeString(String s) {
		int len;
		if (s == null || (len = s.length()) == 0)
			return "";
		return dfs(s, len, 0)[1];
	}
	private String[] dfs(String s, int len, int cur) {
		String[] str = new String[2];
		String res = "";
		int num = 0;
		char ch;
		for (int i = cur; i < len; i++) {
			ch = s.charAt(i);
			if ('0' <= ch && ch <= '9')
				num = num * 10 + (ch - '0');
			else if (ch == '[') { // 遇到左括号，就递归下一部分
				str = dfs(s, len, i + 1);
				for (int j = 1; j <= num; j++)
					res += str[1];
				num = 0;
				i = Integer.valueOf(str[0]); // 当前指针定位到左括号匹配的右括号上
			} else if (ch == ']') { // 若是右括号，说明需要出栈，当前子区间已处理完毕
				str[0] = String.valueOf(i);
				str[1] = res;
				return str;
			} else
				res += ch; // 否则直接相加
		}
		str[1] = res; // 最后将整个答案封装到1位置中
		return str;
	}
}
```