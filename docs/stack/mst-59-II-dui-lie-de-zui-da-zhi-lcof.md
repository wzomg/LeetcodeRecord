# 面试题59-II.队列的最大值
题目链接：[传送门](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

## 题目描述：
请定义一个队列并实现函数`max_value`得到队列里的最大值，要求函数`max_value`、`push_back`和 `pop_front`的均摊时间复杂度都是 $O(1)$。

若队列为空，`pop_front`和`max_value`需要返回`-1`。

**示例**：

- 输入: 
```
["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]
[[],[1],[2],[],[],[]]
```
- 输出: `[null,null,null,2,1,2]`

## 解决方案：
- 时间复杂度：出队操作为 $O(1)$，取最大值为 $O(1)$，入队操作平均为 $O(1)$。
- 空间复杂度：$O(n)$
- 思路：维护一个单调不增的双端队列即可。

## AC代码：
```java
class MaxQueue {
	Queue<Integer> qScan;
	Deque<Integer> qMax;
	public MaxQueue() {
		qScan = new ArrayDeque<Integer>();
		qMax = new ArrayDeque<Integer>();
	}
	public int max_value() {
		return qMax.isEmpty() ? -1 : qMax.element(); // element()方法是返回队首元素
	}
	public void push_back(int value) {
		qScan.add(value);
		while (!qMax.isEmpty() && qMax.getLast() < value) // 维护一个单调不增的双端队列
			qMax.removeLast();
		qMax.addLast(value);
	}
	public int pop_front() {
		if (qScan.isEmpty())
			return -1;
		int val = qScan.remove();
		if (!qMax.isEmpty() && qMax.element() == val)
			qMax.removeFirst();
		return val;
	}
}
```