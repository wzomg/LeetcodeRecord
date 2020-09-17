# 685.冗余连接II
题目链接：[传送门](https://leetcode-cn.com/problems/redundant-connection-ii/)

## 题目描述：
在本问题中，有根树指满足以下条件的有向图。该树只有一个根节点，所有其他节点都是该根节点的后继。每一个节点只有一个父节点，除了根节点没有父节点。

输入一个有向图，该图由一个有着N个节点 (节点值不重复1, 2, ..., N) 的树及**一条附加的边**构成。附加的边的两个顶点包含在1到N中间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组。每一个边的元素是一对`[u, v]`，用以表示**有向图**中连接顶点`u`和顶点`v`的边，其中`u`是`v`的一个父节点。

返回一条能删除的边，使得剩下的图是有N个节点的有根树。若有多个答案，返回最后出现在给定二维数组的答案。

**注意**:

- 二维数组大小的在`3`到`1000`范围内。
- 二维数组中的每个整数在1到N之间，其中`N`是二维数组的大小。

**示例**：

- 输入: `[[1,2], [2,3], [3,4], [4,1], [1,5]]`
- 输出: `[4,1]`
- 解释: 给定的有向图如下：

```
5 <- 1 -> 2
     ^    |
     |    v
     4 <- 3
```

## 解决方案：
- 时间复杂度：$O(n^2 \times \alpha(n))$
- 空间复杂度：$O(n)$
- 思路：每个节点只有一个父节点（入度为1），除了根节点没有父节点（入度为0），因为多了一条附加边，所以可能出现入度为2的节点。先考虑这些入度为2的节点，再考虑入度为1的节点，两者都尝试去删除一条边，若删除后不构成回路，则这条边的2个顶点即为所求。

## AC代码：
```java
class Solution {
	private int[] fa;
	public int[] findRedundantDirectedConnection(int[][] edges) {
		int len;
		if (edges == null || (len = edges.length) == 0)
			return new int[0];
		int[] inDeg = new int[len + 1];
		fa = new int[len + 1];
		// 统计每个节点的入度
		for (int[] edge : edges) {
			inDeg[edge[1]]++;
		}
		// 先处理入度为2的节点，在列表从后往前找，尝试删除一条边，看看是否形成环
		for (int i = len - 1; i >= 0; i--) {
			if (inDeg[edges[i][1]] == 2 && !checkCircle(edges, len, i)) {
				// 若没有形成环，则说明删除当前边是可取的
				return edges[i];
			}
		}
		// 再处理入度为1的节点，同样是从列表后面往前查找
		for (int i = len - 1; i >= 0; i--) {
			if (inDeg[edges[i][1]] == 1 && !checkCircle(edges, len, i)) {
				return edges[i];
			}
		}
		return new int[0];
	}
	private boolean checkCircle(int[][] edges, int len, int removeEdgeIndex) {
		for (int i = 0; i <= len; i++)
			fa[i] = i;
		for (int i = 0; i < len; i++) {
			// 跳过要删除的边
			if (i == removeEdgeIndex)
				continue;
			// 合并2个节点，以下说明形成环
			if (!unite(edges[i][0], edges[i][1])) {
				return true;
			}
		}
		return false;
	}
	private boolean unite(int x, int y) {
		x = find_fa(x);
		y = find_fa(y);
		if (x != y) {
			fa[y] = x;
			return true;
		}
		return false;
	}
	private int find_fa(int x) {
		return fa[x] == x ? x : (fa[x] = find_fa(fa[x]));
	}
}
```