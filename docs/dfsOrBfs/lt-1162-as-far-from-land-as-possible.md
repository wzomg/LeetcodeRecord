# 1162.地图分析
题目链接：[传送门](https://leetcode-cn.com/problems/as-far-from-land-as-possible/)

## 题目描述：
你现在手里有一份大小为 $N \times N$ 的「地图」（网格）`grid`，上面的每个「区域」（单元格）都用`0`和`1`标记好了。其中`0`代表海洋，`1`代表陆地，请你找出一个海洋区域，这个海洋区域到离它最近的陆地区域的距离是最大的。

我们这里说的距离是「曼哈顿距离」（`Manhattan Distance`）：$(x_0, y_0)$ 和 $(x_1, y_1)$ 这两个区域之间的距离是 $|x_0 - x_1| + |y_0 - y_1|$ 。

如果我们的地图上只有陆地或者海洋，请返回 $-1$。

**提示**：

- $1 \leq$ `grid.length == grid[0].length` $ \leq 100$
- `grid[i][j]` 不是`0`就是`1`

**示例1**：

![](../_media/1162_ex1.jpg)

- 输入：`[[1,0,1],[0,0,0],[1,0,1]]`
- 输出：`2`
- 解释：海洋区域 $(1, 1)$ 和所有陆地区域之间的距离都达到最大，最大距离为`2`。


**示例2**：

![](../_media/1162_ex2.jpg)

- 输入：`[[1,0,0],[0,0,0],[0,0,0]]`
- 输出：`4`
- 解释：海洋区域 $(2, 2)$ 和所有陆地区域之间的距离都达到最大，最大距离为`4`。

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n^2)$
- 思路：多源bfs。先求从陆地到海洋的最短距离，然后取海洋位置的最大距离即可。

## AC代码：
```java
class Solution {
	private int n;
	private int INF = 0x3f3f3f3f;
	private int[][] dir = new int[][] { { -1, 0 }, { 0, 1 }, { 1, 0 }, { 0, -1 } };
	public int maxDistance(int[][] grid) {
		if (grid == null || (n = grid.length) == 0)
			return -1;
		int[][] dis = new int[n][n]; // 表示从陆地到海洋的曼哈顿距离
		Queue<Integer> queue = new ArrayDeque<Integer>();
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				if (grid[i][j] == 1) {// 把所有陆地作为源点
					dis[i][j] = 0; // 陆地本身的距离为0
					queue.offer(i * n + j);
				} else
					dis[i][j] = INF;
			}
		}
		int x, y, z, nx, ny, nz, ans = -1; // 注意：ans初始化为-1，避免另做判断
		while (!queue.isEmpty()) {
			z = queue.remove();
			x = z / n;
			y = z % n;
			for (int i = 0; i < 4; i++) {
				nx = x + dir[i][0];
				ny = y + dir[i][1];
				// 肯定不会进入陆地，因为若是陆地，dis[nx][ny]为0，不满足关系式
                // 计算从陆地到海洋的最短距离
				if (check(nx, ny) && dis[nx][ny] > dis[x][y] + 1) { 
					nz = nx * n + ny;
					dis[nx][ny] = dis[x][y] + 1;
					queue.offer(nz);
				}
			}
		}
		for (int i = 0; i < n; i++)
			for (int j = 0; j < n; j++)
				if (grid[i][j] == 0) // 只取从陆地到海洋的最大距离
					ans = Math.max(ans, dis[i][j]);
		// 若只有海洋或只有陆地，则ans为-1
		return ans == INF ? -1 : ans;
	}
	private boolean check(int x, int y) {
		return x >= 0 && x < n && y >= 0 && y < n;
	}
}
```