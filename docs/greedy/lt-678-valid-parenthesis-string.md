# 678.有效的括号字符串
题目链接：[传送门](https://leetcode-cn.com/problems/valid-parenthesis-string/)

## 题目描述：
给定一个只包含三种字符的字符串：`(` ，`)` 和`*`，写一个函数来检验这个字符串是否为有效字符串。有效字符串具有如下规则：

- 任何左括号`(`必须有相应的右括号`)`。
- 任何右括号`)`必须有相应的左括号`(`。
- 左括号`(`必须在对应的右括号之前`)`。
- `*`可以被视为单个右括号`)`，或单个左括号`(`，或一个空字符串。
- 一个空字符串也被视为有效字符串。

**注意**：字符串大小将在 [1，100] 范围内。

**示例**:
- 输入: "(*))"
- 输出: True

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：贪心+栈。用两个栈存放左括号和星号的下标，遇到右括号优先出栈左括号，再出栈星号栈，否则不匹配。遍历完可能剩一些左括号和星号，依次拿左括号栈顶元素和星号栈顶元素进行比较，若前者下标小于后者下标，则弹出左括号栈顶元素（默认一直弹出星号栈顶元素），最后返回左括号栈是否为空即可。

## AC代码：
```go
func checkValidString(s string) bool {
	ltStack, xStack := make([]int, 0), make([]int, 0)
	for k, v := range s {
		if v == '(' {
			ltStack = append(ltStack, k)
		} else if v == '*' {
			xStack = append(xStack, k)
		} else if len(ltStack) > 0 {
			ltStack = ltStack[:len(ltStack)-1] //先出左括号
		} else if len(xStack) > 0 {
			xStack = xStack[:len(xStack)-1] //再出星号
		} else {
			return false
		}
	}
	//剩下左括号和星号
	i := len(ltStack) - 1
	for j := len(xStack) - 1; i >= 0 && j >= 0; j-- { //默认弹出星号栈顶元素
		if ltStack[i] < xStack[j] { //若星号在左括号右边，则弹出左括号栈顶元素
			i--
		}
	}
	return i < 0 //栈中元素必须为空
}
```