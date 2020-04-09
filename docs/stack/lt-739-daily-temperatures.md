# 739.每日温度
题目链接：[传送门](https://leetcode-cn.com/problems/daily-temperatures/)

## 题目描述：
根据每日**气温**列表，请重新生成一个列表，对应位置的输出是需要再等待多久温度才会升高超过该日的天数。如果之后都不会升高，请在该位置用`0`来代替。

例如，给定一个列表`temperatures = [73, 74, 75, 71, 69, 72, 76, 73]`，你的输出应该是`[1, 1, 4, 2, 1, 1, 0, 0]`。

**提示**：**气温**列表长度的范围是`[1, 30000]`。每个气温的值的均为华氏度，都是在`[30, 100]`范围内的整数。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：维护一个单调不增栈（从栈低到栈顶）即可。

## AC代码：
```java
class Solution {
	public int[] dailyTemperatures(int[] T) {
		int len, cur;
		if (T == null || (len = T.length) == 0)
			return new int[0];
		Stack<Integer> stack = new Stack<Integer>();
		int[] res = new int[len];
		for (int i = 0; i < len; i++) {
			while (!stack.isEmpty() && T[stack.peek()] < T[i]) { // 只要当前元素大于栈顶元素才出栈
				cur = stack.pop();
				res[cur] = i - cur;
			}
			stack.push(i);
		}
		return res;
	}
}
```