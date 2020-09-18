# 22.括号生成
题目链接：[传送门](https://leetcode-cn.com/problems/generate-parentheses/)

## 题目描述：
给出`n`代表生成括号的对数，请你写出一个函数，使其能够生成所有可能的并且**有效的**括号组合。

例如，给出`n = 3`，生成结果为：

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

## 解决方案：
- 时间复杂度：求第`n`个卡塔兰数的值。
- 空间复杂度：求第`n`个卡塔兰数的值。
- 思路：简单dfs。（生成含有`n`对合法括号的字符串）。

## AC代码：
```java
class Solution {
	private List<String> res;
	public List<String> generateParenthesis(int n) {
		res = new ArrayList<String>();
		dfs(0, 0, "", n);
		return res;
	}
	private void dfs(int ltCnt, int rtCnt, String comb, int n) {
		// 直接过滤
		if (ltCnt > n || rtCnt > n)
			return;
		// 只有当左括号和右括号的数量都等于n才可以添加生成的字符串
		if (ltCnt == n && rtCnt == n) {
			res.add(comb);
			return;
		}
		// 当左括号的数量小于n才可以添加左括号
		if (ltCnt < n)
			dfs(ltCnt + 1, rtCnt, comb + "(", n);
		// 当左括号的数量大于右括号的数量并且右括号的数量小于n才可以添加右括号
		if (rtCnt < n && ltCnt > rtCnt)
			dfs(ltCnt, rtCnt + 1, comb + ")", n);
	}
}
```