# 680.验证回文字符串Ⅱ
题目链接：[传送门](https://leetcode-cn.com/problems/valid-palindrome-ii/)

## 题目描述：
给定一个非空字符串`s`，最多删除一个字符。判断是否能成为回文字符串。

**注意**:

字符串只包含从`a-z`的小写字母。字符串的最大长度是`50000`。

**示例**:

- 输入: `"abca"`
- 输出: `True`
- 解释: 你可以删除c字符。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：从左往右、从右往左各扫一遍，即消耗一次先删除左边（第一个不相等），若剩下的都匹配成功，则直接返回`true`；否则从右往左遍历，消耗一次删除右边（第一个不相等），若剩下的都匹配成功，则返回`true`，否则返回`false`。

## AC代码：
```java
class Solution {
	public boolean validPalindrome(String s) {
		int len, res;
		if (s == null || (len = s.length()) == 0)
			return true;
		res = 0;
		for (int i = 0, j = len - 1; i < j;) {
			if (s.charAt(i) != s.charAt(j)) {
				i++;
				res++;
				if (res > 1)
					break;
			} else {
				i++;
				j--;
			}
		}
		if (res <= 1)
			return true;
		res = 0;
		for (int i = 0, j = len - 1; i < j;) {
			if (s.charAt(i) != s.charAt(j)) {
				j--;
				res++;
				if (res > 1)
					return false;
			} else {
				i++;
				j--;
			}
		}
		return true;
	}
}
```