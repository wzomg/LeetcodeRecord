# 155.最小栈
题目链接：[传送门](https://leetcode-cn.com/problems/min-stack/)

## 题目描述：
设计一个支持`push`，`pop`，`top`操作，并能在常数时间内检索到最小元素的栈。

- `push(x)` —— 将元素`x`推入栈中。
- `pop()` —— 删除栈顶的元素。
- `top()` —— 获取栈顶元素。
- `getMin()` —— 检索栈中的最小元素。

**提示**：`pop`、`top`和`getMin`操作总是在**非空栈**上调用。

**示例**:

- **输入**：

```
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]
```

- **输出**：`[null,null,null,null,-3,null,0,-2]`

- **解释**：

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

## 解决方案：
- 时间复杂度：$O(1)$
- 空间复杂度：$O(n)$
- 思路：维护一个栈顶最小值的栈即可！

## AC代码：
```java
class MinStack {
	private Stack<Integer> data, minVal;
	public MinStack() {
		data = new Stack<Integer>();
		minVal = new Stack<Integer>();
	}
	public void push(int x) {
		if (data.isEmpty())
			minVal.push(x);
		else
			minVal.push(Math.min(minVal.peek(), x));
		data.push(x);
	}
	public void pop() {
		assert !data.isEmpty();
		data.pop();
		minVal.pop();
	}
	public int top() {
		assert !data.isEmpty();
		return data.peek();
	}
	public int getMin() {
		assert !data.isEmpty();
		return minVal.peek();
	}
}
```