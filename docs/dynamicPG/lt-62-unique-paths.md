# 62.不同路径
题目链接：[传送门](https://leetcode-cn.com/problems/unique-paths/)

## 题目描述：
一个机器人位于一个 $m \times n$ 网格的左上角（起始点在下图中标记为“Start” ）。

机器人每次只能**向下**或者**向右**移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

问总共有多少条不同的路径？

**提示**：

- $ 1 \leq m, n \leq 100 $
- 题目数据保证答案小于等于 $ 2 \times 10^9$

![](../_media/robot_maze.png)

例如，上图是一个 $7 \times 3 $ 的网格。有多少可能的路径？

**示例**:

- 输入: `m = 3`, `n = 2`
- 输出: `3`
- 解释:从左上角开始，总共有 3 条路径可以到达右下角。
    - 1.向右 -> 向右 -> 向下
    - 2.向右 -> 向下 -> 向右
    - 3.向下 -> 向右 -> 向右

## 解决方案：
- 时间复杂度：$O(m \times n)$
- 空间复杂度：$O(m \times n)$
- 思路：简单dp求路径种类数。递推公式：`dp[i][j] = dp[i][j - 1] + dp[i - 1][j];`（其中 $i > 0 \land j > 0$）；当 $i=0 \lor j=0 $ 时，`dp[i][j]=1`。

## AC代码：
```java
class Solution {
	public int uniquePaths(int m, int n) {
		int[][] dp = new int[m][n];
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (i == 0 || j == 0)
					dp[i][j] = 1;
				else
					dp[i][j] = dp[i][j - 1] + dp[i - 1][j];
			}
		}
		return dp[m - 1][n - 1];
	}
}
```
