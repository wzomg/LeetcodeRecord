# 63.不同路径 II
题目链接：[传送门](https://leetcode-cn.com/problems/unique-paths-ii/)

## 题目描述：

一个机器人位于一个 $m \times n$ 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

![](../_media/robot_maze.png)

网格中的障碍物和空位置分别用`1`和`0`来表示。

**说明**：`m`和`n`的值均不超过`100`。

**示例**:

- 输入:

```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```

- 输出: `2`
- 解释: $3 \times 3$ 网格的正中间有一个障碍物。从左上角到右下角一共有`2`条不同的路径：
    - 1.向右 -> 向右 -> 向下 -> 向下
    - 2.向下 -> 向下 -> 向右 -> 向右

## 解决方案：
- 时间复杂度：$O(m \times n)$
- 空间复杂度：$O(m \times n)$
- 思路：动态规划。定义`dp[i][j]`：从坐标`(0,0)`走到`(i,j)`的方案数。很容易想到，当`obstacleGrid[i][j]==0`时，直接跳过；当`i==0`时，`dp[i][j]=dp[i][j-1]`；当`j==0`时，`dp[i][j]=dp[i-1][j]`；其余情况同理可得`dp[i][j]=dp[i-1][j]+dp[i][j-1]`（`i>0 && j>0`）。

## AC代码：
```java
class Solution {
	public int uniquePathsWithObstacles(int[][] obstacleGrid) {
		int m = obstacleGrid.length, n = obstacleGrid[0].length;
		int[][] dp = new int[m][n];
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				if (obstacleGrid[i][j] > 0)
					continue;
				if (i == 0 && j == 0)
					dp[i][j] = 1;
				else if (i == 0)
					dp[i][j] = dp[i][j - 1];
				else if (j == 0)
					dp[i][j] = dp[i - 1][j];
				else
					dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
			}
		}
		return dp[m - 1][n - 1];
	}
}
```
