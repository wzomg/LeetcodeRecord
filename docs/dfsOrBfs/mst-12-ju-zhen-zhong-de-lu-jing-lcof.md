# 面试题12.矩阵中的路径
题目链接：[传送门](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

## 题目描述：
请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串`“bfce”`的路径（路径中的字母用加粗标出）。

```
[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]
```

但矩阵中不包含字符串`“abfb”`的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

**提示**：

- $ 1 \leq $ `board.length` $ \leq 200 $
- $ 1 \leq $ `board[i].length` $ \leq 200 $

**示例**：

- 输入：`board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]`, `word = "ABCCED"`
- 输出：`true`

## 解决方案：
- 时间复杂度：$O(m \times n)$
- 空间复杂度：$O(1)$
- 思路：深搜。首先找到该找的字符串首字母，然后暴力搜索即可！

## AC代码：
```java
class Solution {
	private int[][] dir = new int[][] { { -1, 0 }, { 0, 1 }, { 1, 0 }, { 0, -1 } };
	private int m, n, len;
	private boolean flag;
	public boolean exist(char[][] board, String word) {
		m = board.length;
		n = board[0].length;
		flag = false;
		len = word.length();
		char tmp;
		for (int i = 0; i < m; i++) {
			for (int j = 0; j < n; j++) {
				tmp = board[i][j];
				if (tmp == word.charAt(0)) {
					board[i][j] = '#';
					dfs(board, i, j, 1, word);
					if (flag)
						return true;
					board[i][j] = tmp;
				}
			}
		}
		return false;
	}
	private void dfs(char[][] board, int x, int y, int cur, String word) {
		if (cur == len || flag) {
			flag = true;
			return;
		}
		for (int i = 0; i < 4; i++) {
			int dx = x + dir[i][0];
			int dy = y + dir[i][1];
			if (check(dx, dy) && board[dx][dy] == word.charAt(cur)) {
				char tmp = board[dx][dy];
                // 标记该坐标已走过
				board[dx][dy] = '#';
				dfs(board, dx, dy, cur + 1, word);
                // 还原该字符
				board[dx][dy] = tmp;
			}
		}
	}
	private boolean check(int x, int y) {
		return x >= 0 && x < m && y >= 0 && y < n;
	}
}
```