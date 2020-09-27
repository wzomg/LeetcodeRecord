# 面试题10-I.斐波那契数列
题目链接：[传送门](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

## 题目描述：
写一个函数，输入`n`，求斐波那契（`Fibonacci`）数列的第`n`项。斐波那契数列的定义如下：

```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
```

斐波那契数列由`0`和`1`开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模`1e9+7`（`1000000007`），如计算初始结果为：`1000000008`，请返回`1`。

**提示**：$0 \leq n \leq 100$

**示例**：

- 输入：`n = 5`
- 输出：`5`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：简单递推！

## AC代码：
```java
class Solution {
	public int fib(int n) {
		if (n <= 1)
			return n;
		int a = 0, b = 1, c, mod = 1000000007;
		for (int i = 2; i <= n; i++) {
            // 记得取模
			c = (a + b) % mod; 
			a = b;
			b = c;
		}
		return b;
	}
}
```