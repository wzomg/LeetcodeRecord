# 892.三维形体的表面积
题目链接：[传送门](https://leetcode-cn.com/problems/surface-area-of-3d-shapes/)

## 题目描述：
在 $N \times N$ 的网格上，我们放置一些 $1 \times 1 \times 1$  的立方体。

每个值`v = grid[i][j]`表示`v`个正方体叠放在对应单元格`(i, j)`上。

请你返回最终形体的表面积。

**提示**：

- $1 \leq N \leq 50$
- $0 \leq$ `grid[i][j]` $ \leq 50$

**示例1**：

- 输入：`[[1,1,1],[1,0,1],[1,1,1]]`
- 输出：`32`

**示例2**：

- 输入：`[[2,2,2],[2,1,2],[2,2,2]]`
- 输出：`46`


## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(1)$
- 思路：数学题，减去重合的部分即可。

## AC代码：
```java
class Solution {
	public int surfaceArea(int[][] grid) {
		int n, res = 0;
		if (grid == null || (n = grid.length) == 0)
			return 0;
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				res += grid[i][j] * 6;
				if (j > 0) // 左
					res -= Math.min(grid[i][j - 1], grid[i][j]) << 1;
				if (i > 0) // 后
					res -= Math.min(grid[i - 1][j], grid[i][j]) << 1;
				if (grid[i][j] > 1) // 上
					res -= (grid[i][j] - 1) << 1;
			}
		}
		return res;
	}
}
```