# 面试题09.用两个栈实现队列
题目链接：[传送门](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

## 题目描述：
用两个栈实现一个队列。队列的声明如下，请实现它的两个函数`appendTail`和`deleteHead`，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，`deleteHead`操作返回`-1`)

**提示**：

- $1 \leq values \leq 10000$
- 最多会对`appendTail`、`deleteHead`进行`10000`次调用

**示例**：

- 输入：`["CQueue","appendTail","deleteHead","deleteHead"]`，`[[],[3],[],[]]`
- 输出：`[null,null,3,-1]`

## 解决方案：
- 时间复杂度：插入：$O(1)$，删除：$O(n)$
- 空间复杂度：$O(n)$
- 思路：用一个辅助栈来存放要出队的元素（栈顶），每次要出队时，先检查辅助栈是否为空，若为空，则将存放数据的栈中依次弹出数据并压入辅助栈中；若不为空，则直接弹出辅助栈即可！ 

## AC代码：
```java
class CQueue {
	private Stack<Integer> stack1;
	private Stack<Integer> stack2;
	public CQueue() {
		stack1 = new Stack<Integer>();
		stack2 = new Stack<Integer>();
	}
	public void appendTail(int value) {
		stack1.push(value);
	}
	public int deleteHead() {
        // 当辅助栈为空时，才将存放数据的栈倾倒在辅助栈中
		if (stack2.isEmpty()) {
			while (!stack1.isEmpty())
				stack2.push(stack1.pop());
		}
		return stack2.isEmpty() ? -1 : stack2.pop();
	}
}
```