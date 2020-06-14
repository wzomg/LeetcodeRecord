# 994.腐烂的橘子
题目链接：[传送门](https://leetcode-cn.com/problems/rotting-oranges/)

## 题目描述：
在给定的网格中，每个单元格可以有以下三个值之一：
- 值 $0$ 代表空单元格；
- 值 $1$ 代表新鲜橘子；
- 值 $2$ 代表腐烂的橘子。

每分钟，任何与腐烂的橘子（在 $4$ 个正方向上）相邻的新鲜橘子都会腐烂。

返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 $-1$。

**示例**：

![](../_media/oranges.png)

- 输入：`[[2,1,1],[1,1,0],[0,1,1]]`
- 输出：`4`

**提示**：
- $1 \leq$ `grid.length` $ \leq 10$
- $1 \leq$ `grid[0].length` $\leq 10$
- `grid[i][j]` 仅为 `0`、`1` 或 `2`

## 解决方案：
- 时间复杂度：$O(m \times n)$
- 空间复杂度：$O(m \times n)$
- 思路：多源bfs。将所有腐败的橘子都入队，同时清除标记；初始时腐烂的橘子处时间为0，每次只往新鲜的橘子位置处走即可。有个技巧，如何存储坐标点的位置，把二维数组$(i,j)$看成一维数组的下标来处理即可！

## AC代码：
```java
class Solution {
	private int dir[][] = { { -1, 0 }, { 0, 1 }, { 1, 0 }, { 0, -1 } };
	private int m, n;
	public int orangesRotting(int[][] grid) {
		if (grid == null || (m = grid.length) == 0 || (n = grid[0].length) == 0)
			return 0;
		Queue<Integer> queue = new ArrayDeque<Integer>();
		int[][] dis = new int[m][n]; // 申请一个数组来保存经过的时间
		int cnt1 = 0;
		// ArrayDeque 是JDK容器中的一个双端队列实现，内部使用数组进行元素存储，不允许存储null值，
		// 可以高效的进行元素查找和尾部插入取出，是用作队列、双端队列、栈的绝佳选择，性能比LinkedList还要好。
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
                // 把所有已腐烂的橘子先添加到队列中
				if (grid[i][j] == 2) { 
					queue.add(i * n + j);
                    // 并将其修改为空单元格
					grid[i][j] = 0;
				} else if (grid[i][j] == 1)
                    // 统计新鲜橘子的个数
					cnt1++;
			}
		}
		int cur, x, y, dx, dy, res = 0; // 初始值要为0
        // 若没有新鲜橘子，直接退出循环，不用管队列中是否含有腐烂的橘子
		while (cnt1 > 0 && !queue.isEmpty()) { 
			cur = queue.remove();
			x = cur / n;
			y = cur % n;
			for (int i = 0; i < 4; i++) {
				dx = x + dir[i][0];
				dy = y + dir[i][1];
				if (check(dx, dy) && grid[dx][dy] == 1) { // 若是新鲜橘子
					dis[dx][dy] = dis[x][y] + 1;
					queue.add(dx * n + dy);
                    // 将其变为空单元格
					grid[dx][dy] = 0; 
                    // 因为是按层遍历的，最后走到一个新鲜的橘子时间是肯定是最长的。
					res = dis[dx][dy]; 
					cnt1--;
				}
			}
		}
		return cnt1 > 0 ? -1 : res;
	}
	private boolean check(int x, int y) {
		return x >= 0 && x < m && y >= 0 && y < n;
	}
}
```