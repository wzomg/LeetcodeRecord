# 221.最大正方形
题目链接：[传送门](https://leetcode-cn.com/problems/maximal-square/)

## 题目描述：
在一个由`0`和`1`组成的二维矩阵内，找到只包含`1`的最大正方形，并返回其面积。

**示例**：

- 输入： 

```
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```

- 输出: `4`

## 解决方案：
- 时间复杂度：$O(m \times n)$
- 空间复杂度：$O(n)$
- 思路：动态规划。考虑以`(i,j)`为右下角的正方形最大边长。即`dp[i][j]`：表示以`(i,j)`为右下角的正方形的最大边长，那么需要取左、左上、上位置的最小值`+1`，可得状态转移方程为：`dp[i][j] = min(min(dp[i][j - 1], dp[i - 1][j]), dp[i - 1][j - 1]) +1`。从循环来看，因为当前值只与上一个状态有关，所以二维dp可以压缩成一维dp。

## AC代码：
```java
class Solution {
	public int maximalSquare(char[][] matrix) {
		int m, n, res = 0, pre = 0;
		if (matrix == null || (m = matrix.length) == 0)
			return 0;
		n = matrix[0].length;
		int[] dp = new int[n + 1];
		for (int i = 1, tmp; i <= m; i++) {
			for (int j = 1; j <= n; j++) {
				tmp = dp[j]; // 先保存上一次计算的值，因为当前行计算之后会将其覆盖
				if (matrix[i - 1][j - 1] == '1') { // 当前位置是1才累加
					dp[j] = Math.min(Math.min(dp[j - 1], dp[j]), pre) + 1;
					res = Math.max(res, dp[j]);
				} else // 若当前位置不是1，显然构成正方形的长度为0
					dp[j] = 0;
				pre = tmp; // 再将值赋给pre即为下个(i,j)左上角的值
			}
		}
		return res * res;
	}
}
```
