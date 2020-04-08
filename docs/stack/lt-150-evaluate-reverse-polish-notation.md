# 150. 逆波兰表达式求值
题目链接：[传送门](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/)

## 题目描述：
根据逆波兰表示法，求表达式的值。

有效的运算符包括 `+, -, *, /` 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

说明：
- 整数除法只保留整数部分。
- 给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：逆波兰表达式的求解过程就是遍历字符串数组，若当前元素是数字，则直接入栈；若当前元素是运算符就弹出栈顶2个元素，然后将计算的结果压入栈中，直至最后栈中剩下一个元素就是该表达式计算的结果！

## AC代码：
```java
class Solution {
	public int evalRPN(String[] tokens) {
		int len;
		if (tokens == null || (len = tokens.length) == 0)
			return 0;
		Stack<Integer> stack = new Stack<Integer>();
		for (int i = 0, curLen, curD, a, b; i < len; i++) {
			curLen = tokens[i].length();
			curD = check(tokens[i].charAt(0));
			if (curLen == 1 && curD < 5) { // 运算符判断：长度为1且标志小于5
				b = stack.pop(); // 注意操作数的位置，b先接，a后接
				a = stack.pop();
				switch (curD) {
				case 1:
					stack.add(a + b);
					break;
				case 2:
					stack.add(a - b);
					break;
				case 3:
					stack.add(a * b);
					break;
				case 4:
					stack.add(a / b);
					break;
				}
			} else // 若为数字，直接入栈
				stack.push(Integer.parseInt(tokens[i]));
		}
		return stack.pop(); // 返回该表达式的结果！
	}
	private int check(char ch) {
		if (ch == '+')
			return 1;
		else if (ch == '-')
			return 2;
		else if (ch == '*')
			return 3;
		else if (ch == '/')
			return 4;
		else
			return 5;
	}
}
```