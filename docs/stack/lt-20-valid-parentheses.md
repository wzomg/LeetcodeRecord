# 20.有效的括号
题目链接：[传送门](https://leetcode-cn.com/problems/valid-parentheses/)

## 题目描述：
给定一个只包括`'('`，`')'`，`'{'`，`'}'`，`'['`，`']'`的字符串，判断字符串是否有效。

有效字符串需满足：

- 左括号必须用相同类型的右括号闭合。
- 左括号必须以正确的顺序闭合。

**注意**：空字符串可被认为是有效字符串。

**示例**：

- 输入: `"()[]{}"`
- 输出: `true`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：简单栈的运用。

## AC代码：
```java
class Solution {
	public boolean isValid(String s) {
		Stack<Character> stack = new Stack<Character>();
		boolean flag = true;
		char ch;
		for (int i = 0, len = s.length(), op; i < len; i++) {
			ch = s.charAt(i);
			if (ch == '(' || ch == '{' || ch == '[') {
				stack.push(s.charAt(i));
			} else if (!stack.empty()) {
				switch (stack.peek()) {
				case '(':
					op = 1;
					break;
				case '{':
					op = 2;
					break;
				default:
					op = 3;
					break;
				}
				if (!((s.charAt(i) == ')' && op == 1) || (s.charAt(i) == '}' && op == 2)
						|| (s.charAt(i) == ']' && op == 3))) {
					flag = false;
					break;
				} else {
					stack.pop();
				}
			} else {
				flag = false;
				break;
			}
		}
		// 注意，最后栈要为空！
		return stack.empty() && flag;
	}
}
```