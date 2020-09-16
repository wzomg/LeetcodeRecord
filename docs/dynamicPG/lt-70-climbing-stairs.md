# 70.爬楼梯
题目链接：[传送门](https://leetcode-cn.com/problems/climbing-stairs/)

## 题目描述：
假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 `n` 是一个正整数。

**示例**：

- 输入： `3`
- 输出： `3`
- 解释： 有三种方法可以爬到楼顶。

```
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：简单求斐波那契数列。

## AC代码：
```java
class Solution {
	public int climbStairs(int n) {
		int a = 1, b = 1;
		for (int i = 2, c; i <= n; i++) {
			c = a + b;
			a = b;
			b = c;
		}
		return b;
	}
}
```