# 10.正则表达式匹配
题目链接：[传送门](https://leetcode-cn.com/problems/regular-expression-matching/)

## 题目描述：
给你一个字符串`s`和一个字符规律`p`，请你来实现一个支持`'.'`和`'*'`的正则表达式匹配。

```
'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
```

所谓匹配，是要涵盖**整个**字符串`s`的，而不是部分字符串。

**说明**：

- `s`可能为空，且只包含从`a-z`的小写字母。
- `p`可能为空，且只包含从`a-z`的小写字母，以及字符`.`和`*`。

**示例**：

- 输入：`s = "ab"`，`p = ".*"`
- 输出: `true`
- 解释: `".*"`表示可匹配零个或多个（`'*'`）任意字符（`'.'`）。

## 解决方案：
- 时间复杂度：$O(m \times n)$
- 空间复杂度：$O(m \times n)$
- 思路：定义`dp[i][j]`为`s`的前`i`个字符是否都能被`p`字符串中的前`j`个字符匹配，自底向上从子问题递推到大问题即可。

## AC代码1：暴力dfs。
```java
class Solution {
	public boolean isMatch(String s, String p) {
		// 终止条件：若模式串为空，并且s字符必须为空才匹配
		if (p.isEmpty())
			return s.isEmpty();
		if (s.isEmpty()) {
			// 如果s字符串为空，但是p可能不为空，如：p剩下b*等
			// 若p的长度不小于2，并且p[1]=='*'，则直接舍弃当前部分 isMatch(s, p[2:])
			if (p.length() > 1 && p.charAt(1) == '*')
				return isMatch(s, p.substring(2));
			// 若p的长度为1，或者p的下一个字符不是*，显然是不匹配的
			return false;
		}
		// 判断当前位置的字符，i，j，前提是s字符串不为空
		else if (s.charAt(0) == p.charAt(0) || p.charAt(0) == '.') {
            // 如果对应位置的字符相同，那就判断下一个字符是否为* 
			// 若p模式串还有下一个字符，且为*
			// 分3种情况：
			// ①*为0次匹配，如abb、a*abb --> isMatch(s, p[2:])
			// ②*为1次匹配，如abb、a*bb --> isMatch(s[1:], p[2:])
			// ③*为多次匹配，如aaab、a*b --> isMatch(s[1:], p);

			// 若当前p[0]为.，则判断下一个字符是否为*
			// 若为*，也分3种情况，讨论*取的个数
			// ① 若*为0次匹配，如：abb、.*abb --> isMatch(s, p[2:])
			// ② 若*为1次匹配，如：abb、.*bb --> isMatch(s[1:], p[2:])
			// ③ 若*为多次匹配，如：aaab、.* --> isMatch(s[1:], p)
			if (p.length() > 1 && p.charAt(1) == '*') {
				return isMatch(s, p.substring(2))/* || isMatch(s.substring(1), p.substring(2)) */
						|| isMatch(s.substring(1), p);
			}
			// 若p剩下长度为1，或者p的下一个字符不是*，则直接递归下一次比较
			// 若p剩下长度为1，或者p的下一个字符不是*，则直接递归下一次比较，因为'.'只匹配单个字符
			return isMatch(s.substring(1), p.substring(1));
		} else {
			// 其余情况不相等，先看下一个字符是否为*，若是则直接取*匹配0个 --> isMatch(s, p[2:])
			if (p.length() > 1 && p.charAt(1) == '*')
				return isMatch(s, p.substring(2));
			// 若p剩下长度为1，或者p的下一个字符不是*，则直接返回false
			return false;
		}
	}
}
```

## AC代码2：动态规划。
```java
class Solution {
	public boolean isMatch(String s, String p) {
		int m, n;
		if (s == null && p == null)
			return true;
		else if (s == null || p == null)
			return false;
		m = s.length();
		n = p.length();
		boolean[][] dp = new boolean[m + 1][n + 1]; // ！
		for (int i = m; i >= 0; i--) {
			for (int j = n; j >= 0; j--) {
				if (i == m && j == n) // 表示都是空字符串
					dp[i][j] = true;
				else if (i == m) // s为空串
					dp[i][j] = (j + 1 < n && p.charAt(j + 1) == '*') ? dp[i][j + 2] : false;
				else if (j < n) {
					if (s.charAt(i) == p.charAt(j) || p.charAt(j) == '.') {
						if (j + 1 < n && p.charAt(j + 1) == '*')
							dp[i][j] = dp[i][j + 2] | dp[i + 1][j];
						dp[i][j] |= dp[i + 1][j + 1];
					} else
						dp[i][j] = (j + 1 < n && p.charAt(j + 1) == '*') ? dp[i][j + 2] : false;
				}
			}
		}
		return dp[0][0];
	}
}
```