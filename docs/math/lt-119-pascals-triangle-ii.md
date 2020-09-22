# 119.杨辉三角II
题目链接：[传送门](https://leetcode-cn.com/problems/pascals-triangle-ii/)

## 题目描述：
给定一个非负索引`k`，其中 $ k\leq 33$，返回杨辉三角的第`k`行。

在杨辉三角中，每个数是它左上方和右上方的数的和。

**进阶**：你可以优化你的算法到 $O(k)$ 空间复杂度吗？

**示例**:

- 输入：`3`
- 输出：`[1,3,3,1]`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：简单排列组合。根据二项式定理公式可得杨辉三角的第`n`行各项的系数依次为$C_n ^0, C_n ^1, C_n ^2, \cdots, C_n ^n$，遍历一次同时计算即可，注意乘法溢出。

![](../_media/二项式定理公式.svg)

## AC代码：
```java
class Solution {
	public List<Integer> getRow(int rowIndex) {
		List<Integer> res = new ArrayList<Integer>();
		res.add(0);
		long cur;
		for (int i = 1; i <= rowIndex; i++) {
			cur = (long) res.get(i - 1) * (rowIndex - i + 1) / i;
			res.add((int) cur);
		}
		return res;
	}
}
```