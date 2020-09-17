# 684.冗余连接
题目链接：[传送门](https://leetcode-cn.com/problems/redundant-connection/)

## 题目描述：
在本问题中, 树指的是一个连通且无环的**无向**图。

输入一个图，该图由一个有着N个节点 (节点值不重复1, 2, ..., N) 的树及一条附加的边构成。附加的边的两个顶点包含在1到N中间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组。每一个边的元素是一对`[u, v]`，满足 $u < v$，表示连接顶点u 和v的无向图的边。

返回一条可以删去的边，使得结果图是一个有着N个节点的树。如果有多个答案，则返回二维数组中最后出现的边。答案边`[u, v]`应满足相同的格式 $u < v$。

**注意**：

- 输入的二维数组大小在 `3` 到 `1000`。
- 二维数组中的整数在1到N之间，其中`N`是输入数组的大小。

**示例**：

- 输入: `[[1,2], [2,3], [3,4], [1,4], [1,5]]`
- 输出: `[1,4]`
- 解释: 给定的无向图为：

```
5 - 1 - 2
    |   |
    4 - 3
```

## 解决方案：
- 时间复杂度：$O(n \times \alpha(n))$
- 空间复杂度：$O(n)$
- 思路：并查集。按顺序合并节点，若相连的2个节点具有相同的父节点，则应删除这条边，否则将构成环。注意：题目中给的是无向图。

## AC代码：
```java
class Solution {
	private int[] fa;
	public int[] findRedundantConnection(int[][] edges) {
		int len;
		if (edges == null || (len = edges.length) == 0)
			return new int[0];
		fa = new int[len + 1];
		for (int i = 0; i <= len; i++)
			fa[i] = i;
		int res = 0;
		for (int i = 0, x, y; i < len; i++) {
			x = edges[i][0];
			y = edges[i][1];
			x = find_fa(x);
			y = find_fa(y);
			if (x != y)
				fa[y] = x;
			else // 不 break 掉，继续遍历，找出最后出现的边
				res = i;
		}
		return edges[res];
	}
	private int find_fa(int x) {
		return fa[x] == x ? x : (fa[x] = find_fa(fa[x]));
	}
}
```