# 120.三角形最小路径和
题目链接：[传送门](https://leetcode-cn.com/problems/triangle/)

## 题目描述：
给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。

**相邻的结点**在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。

- 例如，给定三角形：
```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```
- 自顶向下的最小路径和为 11（即，$2 + 3 + 5 + 1 = 11$）。

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n)$
- 思路：简单动态规划，先初始化边界值，再自底向上递推即可。

## AC代码：
```java
class Solution {
	public int minimumTotal(List<List<Integer>> triangle) {
		int n = triangle.size();
		int[] dp = new int[n];
		for (int j = 0; j < n; j++)
			dp[j] = triangle.get(n - 1).get(j);
		for (int i = n - 2; i >= 0; i--) {
			for (int j = 0, siz = triangle.get(i).size(); j < siz; j++) {
				dp[j] = Math.min(dp[j], dp[j + 1]) + triangle.get(i).get(j);
			}
		}
		return dp[0];
	}
}
```