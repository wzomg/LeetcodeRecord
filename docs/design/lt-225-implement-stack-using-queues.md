# 225.用队列实现栈
题目链接：[传送门](https://leetcode-cn.com/problems/implement-stack-using-queues/)

## 题目描述：
使用队列实现栈的下列操作：

- `push(x)` -- 元素 x 入栈
- `pop()` -- 移除栈顶元素
- `top()` -- 获取栈顶元素
- `empty()` -- 返回栈是否为空

**注意**:

- 你只能使用队列的基本操作-- 也就是`push to back`，`peek/pop from front`，`size`和`is empty`这些操作是合法的。
- 你所使用的语言也许不支持队列。你可以使用`list`或者`deque`（双端队列）来模拟一个队列，只要是标准的队列操作即可。
- 你可以假设所有操作都是有效的（例如, 对一个空的栈不会调用`pop`或者`top`操作）。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：用一个单向队列来实现，每次push先添加到队尾，然后将之前的数字依次出队并入队，这样保证队首到队尾依次为栈顶到栈底的元素顺序。

## AC代码：
```java
class MyStack {
	Queue<Integer> queue; // 使用Queue接口，表示FIFO队列
	public MyStack() {
		queue = new LinkedList<Integer>(); // 用LinkedList子集合来实现单向队列
	}
	public void push(int x) {
		queue.add(x); // 队尾添加
		int siz = queue.size();
		while (--siz > 0) // siz-1次
			queue.add(queue.remove()); // 使用remove()会抛出异常，poll()则不会
	}
	public int pop() {
		return queue.remove();
	}
	public int top() {
		return queue.element(); // 使用element()会抛出异常，使用peek()不会抛出异常
	}
	public boolean empty() {
		return queue.isEmpty();
	}
}
```