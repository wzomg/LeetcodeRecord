# 1572.矩阵对角线元素的和
题目链接：[传送门](https://leetcode-cn.com/problems/matrix-diagonal-sum/)

## 题目描述：
给你一个正方形矩阵`mat`，请你返回矩阵对角线元素的和。

请你返回在矩阵主对角线上的元素和副对角线上且不在主对角线上元素的和。

**示例**：

![](../_media/sample_1911.png)

- 输入：

```
mat = [[1,2,3],
       [4,5,6],
       [7,8,9]]
```

- 输出：`25`
- 解释：对角线的和为：`1 + 5 + 9 + 3 + 7 = 25`，请注意，元素`mat[1][1] = 5`只会被计算一次。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：简单数学，当`n`为奇数时减去最中间被重复计算的元素即可。

## AC代码：
```java
class Solution {
	public int diagonalSum(int[][] mat) {
		int n, res = 0;
		if (mat == null || (n = mat.length) == 0)
			return 0;
		for (int i = 0; i < n; i++) {
			res += mat[i][i] + mat[i][n - 1 - i];
		}
		if (n % 2 == 1)
			res -= mat[n / 2][n / 2];
		return res;
	}
}
```