# 378.有序矩阵中第K小的元素
题目链接：[传送门](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/)

## 题目描述：
给定一个 $n \times n $ 矩阵，其中每行和每列元素均按升序排序，找到矩阵中第`k`小的元素。
请注意，它是排序后的第`k`小元素，而不是第`k`个不同的元素。

**提示**：你可以假设`k`的值永远是有效的，$1 \leq k \leq n^2$。

## 解决方案：
- 时间复杂度：$O(n \times log^n)$
- 空间复杂度：$O(n)$
- 思路：这道题的思路和 “合并k个排序链表” 的思路类似，即维护一个最小堆，出队 $ k - 1 $ 个元素，那么第 `k` 个出队的元素就是第 k 小元素。

## AC代码：
```java
class Solution {
	public int kthSmallest(int[][] matrix, int k) {
		int n, m;
		if (matrix == null || (n = matrix.length) == 0 || (m = matrix[0].length) == 0)
			return -1;
		// 维护一个小顶堆
		Queue<int[]> queue = new PriorityQueue<>(new Comparator<int[]>() {
			@Override
			public int compare(int[] A, int[] B) {
				return A[0] - B[0];
			}
		});
		for (int i = 0; i < n; i++)
			queue.add(new int[] { matrix[i][0], i, 0 });
		int[] tmp;
		// 出队 k - 1 次，第 k 个出队就是第 k 小元素
		for (int i = 0; i < k - 1; i++) {
			tmp = queue.remove();
			// 注意：不能是最后一列
			if (tmp[2] != n - 1)
				queue.add(new int[] { matrix[tmp[1]][tmp[2] + 1], tmp[1], tmp[2] + 1 });
		}
		return queue.remove()[0];
	}
}
```