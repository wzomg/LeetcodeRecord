# 面试题31.栈的压入、弹出序列
题目链接：[传送门](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

## 题目描述：
输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 `{1,2,3,4,5}` 是某栈的压栈序列，序列 `{4,5,3,2,1}` 是该压栈序列对应的一个弹出序列，但 `{4,3,5,1,2}` 就不可能是该压栈序列的弹出序列。

**提示**：

- $ 0 \leq $ `pushed.length` == `popped.length` $ \leq 1000 $
- $ 0 \leq$ `pushed[i]`, `popped[i]` $ < 1000$
- `pushed`是`popped`的排列。

**示例**：

- 输入：`pushed = [1,2,3,4,5]`, `popped = [4,5,3,2,1]`
- 输出：`true`
- 解释：我们可以按以下顺序执行：

```
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
```

## 解决方案：
- 时间复杂度：$O(n)$，每个元素至多入栈出栈一次
- 空间复杂度：$O(n)$
- 思路：使用一个辅助栈来模拟实际出栈序列，若最后栈不为空，则该出栈序列肯定不合法！即每次先将元素入栈，若栈顶元素和当前出栈首元素相同，则出栈，否则继续压栈，最后检查一下栈是否为空！

## AC代码：
```java
class Solution {
	public boolean validateStackSequences(int[] pushed, int[] popped) {
		int len;
		if (pushed == null || (len = pushed.length) == 0)
			return true;
		Stack<Integer> stack = new Stack<Integer>();
		for (int i = 0, j = 0; i < len; i++) {
			stack.push(pushed[i]);
			while (!stack.isEmpty() && stack.peek() == popped[j]) {
				stack.pop();
				j++;
			}
		}
		return stack.isEmpty();
	}
}
```