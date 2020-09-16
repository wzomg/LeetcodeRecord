# 51.N皇后
题目链接：[传送门](https://leetcode-cn.com/problems/n-queens/)

## 题目描述：
$n$ 皇后问题研究的是如何将 $n$ 个皇后放置在 $n \times n$ 的棋盘上，并且使皇后彼此之间不能相互攻击。

![](../_media/8-queens.png)

上图为 8 皇后问题的一种解法。

给定一个整数 `n`，返回所有不同的 `n` 皇后问题的解决方案。

每一种解法包含一个明确的 `n` 皇后问题的棋子放置方案，该方案中 `'Q'` 和 `'.'` 分别代表了皇后和空位。

## 解决方案：
- 时间复杂度：$O(n!)$
- 空间复杂度：$O(n)$
- 思路：N皇后问题即任两个皇后都不能处于同一条横行、纵行或斜线上。求解这一问题涉及到试探+回溯算法，用递归就可以将思路清晰地展现出来。我们在试探的过程中，皇后的放置需要检查他的位置是否和已经放置好的皇后发生冲突，为此需要以及检查函数`place()`来检查当前要放置皇后的位置，是不是和其他已经放置的皇后发生冲突。假设有两个皇后被放置在 $(i, j)$ 和 $(k, l)$ 的位置上，明显，当且仅当 $|i-k|=|j-l|$ 时，两个皇后才在同一条对角线上。（1）先从首位开始检查，如果不能放置，接着检查该行第二个位置，依次检查下去，直到在该行找到一个可以放置一个皇后的地方，然后保存当前状态，转到下一行重复上述方法的检索。（2）如果检查了该行所有的位置均不能放置一个皇后，说明上一行皇后放置的位置无法让所有的皇后找到自己合适的位置，因此就要回溯到上一行，重新检查该皇后位置后面的位置。

## AC代码：
```java
class Solution {
	public List<List<String>> solveNQueens(int n) {
		int[] queue = new int[n];
		List<List<String>> res = new ArrayList<List<String>>();
		queueSet(0, n, queue, res);
		return res;
	}
	// 检查横列和对角线上是否可以放置皇后
	private void queueSet(int row, int n, int[] queue, List<List<String>> res) {
		for (int col = 0; col < n; col++) {
			// 将皇后摆到当前循环到的位置
			queue[row] = col;
			// 如果放置成功
			if (place(row, queue)) {
				// 已放置 n 个皇后
				if (row == n - 1) {
					StringBuilder sb;
					List<String> single = new ArrayList<String>();
					for (int i = 0; i < n; i++) {
						sb = new StringBuilder();
						for (int j = 0; j < n; j++)
							sb.append(queue[i] == j ? 'Q' : '.');
						single.add(sb.toString());
					}
					res.add(single);
				} else // 否则继续摆放下一个皇后
					queueSet(row + 1, n, queue, res);
			}
		}
	}
	// 和已经摆放好的皇后进行比较
	private boolean place(int row, int[] queue) {
		for (int i = 0; i < row; i++) {
			if (queue[i] == queue[row] || Math.abs(queue[row] - queue[i]) == Math.abs(row - i))
				return false;
		}
		return true;
	}
}
```