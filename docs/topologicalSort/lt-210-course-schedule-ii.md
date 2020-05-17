# 210.课程表II
题目链接：[传送门](https://leetcode-cn.com/problems/course-schedule-ii/)

## 题目描述：
现在你总共有`n`门课需要选，记为`0`到`n-1`。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程`0`，你需要先完成课程`1`，我们用一个匹配来表示他们：`[0,1]`

给定课程总量以及它们的先决条件，返回你为了学完所有课程所安排的学习顺序。

可能会有多个正确的顺序，你只要返回一种就可以了。如果不可能完成所有课程，返回一个**空数组**。

**说明**:

输入的先决条件是由边缘列表表示的图形，而不是邻接矩阵。
你可以假定输入的先决条件中**没有重复的边**。

**提示**:

- 这个问题相当于查找一个循环是否存在于有向图中。如果存在循环，则不存在拓扑排序，因此不可能选取所有课程进行学习。
- 通过 DFS 进行拓扑排序，也可以通过 BFS 完成。

**示例**:

- 输入: `2`, `[[1,0]]`
- 输出: `[0,1]`
- 解释: 总共有`2`门课程。要学习课程`1`，你需要先完成课程`0`。因此，正确的课程顺序为`[0,1]`。

## 解决方案：
- 时间复杂度：$O(n+e)$
- 空间复杂度：$O(n)$
- 思路：拓扑排序。所谓的拓扑排序就是将入度为0的节点编号入队，当队首元素出队时，依次以它为尾的弧的另一端节点入度减1，同样只要有节点的入度减完为0就将其入队，循环直至不存在无前驱的节点。若出队节点个数小于总节点数，则图中一定存在环。

## AC代码：
```java
class Solution {
	public int[] findOrder(int numCourses, int[][] prerequisites) {
		int[] InDeg = new int[numCourses];
		List<Integer>[] graph = new ArrayList[numCourses];
		for (int i = 0; i < numCourses; i++)
			graph[i] = new ArrayList<>();
		int len = prerequisites.length;
		for (int i = 0; i < len; i++) { // 建图：下标值指向：1 -> 0
			InDeg[prerequisites[i][0]]++;
			graph[prerequisites[i][1]].add(prerequisites[i][0]); // 先修课程指向后修课程
		}
		Queue<Integer> queue = new ArrayDeque<>();
		for (int i = 0; i < numCourses; i++)
			if (InDeg[i] == 0)
				queue.add(i);
		int cnt = 0, cur;
		int[] res = new int[numCourses];
		while (!queue.isEmpty()) {
			cur = queue.remove();
			res[cnt++] = cur;
			for (int j = 0, to, siz = graph[cur].size(); j < siz; j++) {
				to = graph[cur].get(j);
				if (--InDeg[to] == 0)
					queue.add(to);
			}
		}
		return cnt == numCourses ? res : new int[0];
	}
}
```
