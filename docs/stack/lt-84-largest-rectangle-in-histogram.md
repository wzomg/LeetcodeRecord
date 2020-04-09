# 84.柱状图中最大的矩形
题目链接：[传送门](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

## 题目描述：
给定`n`个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为`1`。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

![示例](../_media/histogram.png)

以上是柱状图的示例，其中每个柱子的宽度为`1`，给定的高度为`[2,1,5,6,2,3]`。

![示例解法](../_media/histogram_area.png)

图中阴影部分为所能勾勒出的最大矩形面积，其面积为`10`个单位。

## 解决方案：
- 时间复杂度：$O(n)$，每个元素会被压栈弹栈各一次。
- 空间复杂度：$O(n)$
- 思路：单调栈的运用。

> 个人的详细思路：很容易想到的一点是以每个高度为中心，分别遍历其向左向右的最大延申长度（敲重点！以下讨论始终围绕这一点！）。但这需要$O(n^2)$时间复杂度！有没有$O(n)$遍历呢？有这么一个巧妙的解法：维护一个单调递增栈（栈中元素为每个高度存放的下标）。可以发现，只要有一个高度比当前栈顶元素小，说明栈中某些高度向右延申的长度受限于当前高度，当前高度的下标为记为i，于是这等同于二重循环暴力一样统计延申长度，不过我们可以$O(1)$得到受限高度能向左或向右延申的最大长度，也就是先弹出栈顶元素，下标记为`cur`，值为`heights[cur]`，作为当前要计算矩形的高。那么下标为`cur`的高度向左最大能延申到的下标是多少呢？因为是单调递增栈，显然其向左受限于当前栈顶元素，下标记为`pre`，于是就轻松得到当前要计算的矩形的宽度`(i - 1 - pre)`（栈顶元素的高只要不小于当前下标为`i`的值，其向右延申的长度都受限于下标为`i`的元素），两者相乘更新一下最大矩形面积即可！还有一点，假设高度都是递增的，或者部分高度是递增，即栈中还有元素？同样道理，因为是单调递增栈，显然当前元素向右边延申的长度受限于终止坐标：`len`（`len`为数组长度），向左延申的长度受限于弹出栈顶元素`cur`后的栈顶元素：`pre`，于是可以得到要计算的矩形宽度`(len - 1 - pre)`。最后要注意：若栈为空，则需置`pre`为`-1`，也就是其向左能延申的最大长度到下标为`0`，`0`的前一个位置肯定是`-1`，这样便于计算！

## AC代码：
```java
class Solution {
	public int largestRectangleArea(int[] heights) {
		int len, res = 0, pre, cur;
		if (heights == null || (len = heights.length) == 0)
			return 0;
		Stack<Integer> stack = new Stack<Integer>();
		for (int i = 0; i < len; i++) {
			while (!stack.isEmpty() && heights[stack.peek()] >= heights[i]) {
				cur = stack.pop();
				pre = stack.isEmpty() ? -1 : stack.peek();
				res = Math.max(res, (i - 1 - pre) * heights[cur]);
			}
			stack.push(i);
		}
		while (!stack.isEmpty()) {
			cur = stack.pop();
			pre = stack.isEmpty() ? -1 : stack.peek();
			res = Math.max(res, (len - 1 - pre) * heights[cur]);
		}
		return res;
	}
}
```