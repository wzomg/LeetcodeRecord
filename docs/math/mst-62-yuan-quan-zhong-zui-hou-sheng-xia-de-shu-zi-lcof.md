# 面试题62.圆圈中最后剩下的数字
题目链接：[传送门](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

## 题目描述：
$0,1, \cdots ,n-1$ 这 $n$ 个数字排成一个圆圈，从数字 $0$ 开始，每次从这个圆圈里删除第 $m$ 个数字。求出这个圆圈里剩下的最后一个数字。

例如，`0、1、2、3、4` 这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是`2、0、4、1`，因此最后剩下的数字是3。

**限制**：

- $1 \leq n \leq 10^5$
- $1 \leq m \leq 10^6$

**示例**：

- 输入: `n = 10, m = 17`
- 输出: `2`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：约瑟夫环问题。用公式法做即可。

## AC代码：
```java
class Solution {
	public int lastRemaining(int n, int m) {
		int res = 0;
		for (int i = 2; i <= n; i++) // 从2开始，到n
			res = (res + m) % i;
		return res;
	}
}
```