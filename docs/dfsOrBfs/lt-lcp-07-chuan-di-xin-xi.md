# LCP07.传递信息
题目链接：[传送门](https://leetcode-cn.com/problems/chuan-di-xin-xi/)

## 题目描述：
小朋友`A`在和`ta`的小伙伴们玩传信息游戏，游戏规则如下：

- 1、有`n`名玩家，所有玩家编号分别为`0`～`n-1`，其中小朋友`A`的编号为`0`
- 2、每个玩家都有固定的若干个可传信息的其他玩家（也可能没有）。传信息的关系是单向的（比如`A`可以向`B`传信息，但`B`不能向`A`传信息）。
- 3、每轮信息必须需要传递给另一个人，且信息可重复经过同一个人。

给定总玩家数`n`，以及按 `[玩家编号,对应可传递玩家编号]` 关系组成的二维数组`relation`。返回信息从小`A`(编号`0`)经过`k`轮传递到编号为`n-1`的小伙伴处的方案数；若不能到达，返回`0`。

**限制**：

- $2 \leq n \leq 10$
- $1 \leq k \leq 5$
- $1 \leq$ `relation.length` $\leq 90$, 且 `relation[i].length == 2`
- `0 <= relation[i][0],relation[i][1] < n 且 relation[i][0] != relation[i][1]`

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(m)$
- 思路：数据较小，直接暴搜。先记录将当前玩家可传递的玩家，然后暴搜统计统计方案数即可！

## AC代码：
```java
class Solution {
	private int res;
	private List<Integer>[] list;
	public int numWays(int n, int[][] relation, int k) {
		res = 0;
		list = new ArrayList[n];
		for (int i = 0; i < n; i++)
			list[i] = new ArrayList<Integer>();
		// 记录将当前玩家可传递的玩家
		for (int i = 0; i < relation.length; i++)
			list[relation[i][0]].add(relation[i][1]);
		dfs(n - 1, relation, k, 0);
		return res;
	}
	private void dfs(int n, int[][] relation, int k, int cur) {
		if (k < 0)
			return;
		if (k == 0 && cur == n) {
			++res;
			return;
		}
		for (int i = 0, len = list[cur].size(); i < len; i++)
			dfs(n, relation, k - 1, list[cur].get(i));
	}
}
```