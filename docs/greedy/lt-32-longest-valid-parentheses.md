# 32.最长有效括号
题目链接：[传送门](https://leetcode-cn.com/problems/longest-valid-parentheses/)

## 题目描述：
给定一个只包含`'('`和`')'`的字符串，找出最长的包含有效括号的子串的长度。

**示例**：

- 输入: `")()())"`
- 输出: `4`
- 解释: 最长有效括号子串为 `"()()"`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：贪心。正序遍历：当右括号数量大于左括号数量时，重置左计数和右计数为0；反序遍历：当左括号数量大于右括号数量时，重置左计数和右计数为0；只有当左括号数量和右括号数量相等时，才取一下最大值。为什么还需要反序再遍历一次呢？举个例子：`()((())`，若正序遍历则得到的值为2；而反序遍历得到的值为4，正确的结果为4。

## AC代码：
```java
class Solution {
	public int longestValidParentheses(String s) {
		int len, maxLen = 0, ltCnt = 0, rtCnt = 0;
		if (s == null || (len = s.length()) == 0)
			return 0;
		char ch;
		for (int i = 0; i < len; i++) {
			ch = s.charAt(i);
			if (ch == '(')
				ltCnt++;
			else
				rtCnt++;
			if (ltCnt == rtCnt)
				maxLen = Math.max(maxLen, ltCnt);
			else if (rtCnt > ltCnt)
				ltCnt = rtCnt = 0;
		}
		ltCnt = rtCnt = 0;
		for (int i = len - 1; i >= 0; i--) {
			ch = s.charAt(i);
			if (ch == '(')
				ltCnt++;
			else
				rtCnt++;
			if (ltCnt == rtCnt)
				maxLen = Math.max(maxLen, ltCnt);
			else if (ltCnt > rtCnt)
				ltCnt = rtCnt = 0;
		}
		return maxLen << 1;
	}
}
```