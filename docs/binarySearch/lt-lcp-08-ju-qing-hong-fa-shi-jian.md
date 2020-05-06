# LCP08.剧情触发时间
题目链接：[传送门](https://leetcode-cn.com/problems/ju-qing-hong-fa-shi-jian/)

## 题目描述：
在战略游戏中，玩家往往需要发展自己的势力来触发各种新的剧情。一个势力的主要属性有三种，分别是文明等级（`C`），资源储备（`R`）以及人口数量（`H`）。在游戏开始时（第`0`天），三种属性的值均为`0`。

随着游戏进程的进行，每一天玩家的三种属性都会对应增加，我们用一个二维数组`increase`来表示每天的增加情况。这个二维数组的每个元素是一个长度为`3`的一维数组，例如`[[1,2,1],[3,4,2]]`表示第一天三种属性分别增加`1,2,1`而第二天分别增加`3,4,2`。

所有剧情的触发条件也用一个二维数组`requirements`表示。这个二维数组的每个元素是一个长度为`3`的一维数组，对于某个剧情的触发条件`c[i]`,`r[i]`,`h[i]`，如果当前`C >= c[i]`且`R >= r[i]`且`H >= h[i]`，则剧情会被触发。

根据所给信息，请计算每个剧情的触发时间，并以一个数组返回。如果某个剧情不会被触发，则该剧情对应的触发时间为`-1`。

## 解决方案：
- 时间复杂度：$O(n \times log^n)$
- 空间复杂度：$O(n)$
- 思路：二分查找。先统计一下前缀和，再遍历条件数组，依次找每项中不小于`requirements[i][j]`的最小下标，然后取3者中最大下标即可。注意特判`requirements[i]`中3项值是否都为0，若是则其触发时间为`0`。

## AC代码：
```java
class Solution {
	public int[] getTriggerTime(int[][] increase, int[][] requirements) {
		int len1 = increase.length, len2 = requirements.length, maxIndex;
		for (int i = 1; i < len1; i++)
			for (int j = 0; j < 3; j++)
				increase[i][j] += increase[i - 1][j];
		int[] res = new int[len2];
		int[] col = new int[3];
		for (int i = 0, sum; i < len2; i++) {
			maxIndex = 0;
			sum = 0;
			for (int j = 0; j < 3; j++) {
				col[j] = binarySearch(increase, len1, j, requirements[i][j]);
				maxIndex = Math.max(maxIndex, col[j]);
				sum += requirements[i][j];
			}
			res[i] = sum == 0 ? 0 : (maxIndex == len1 ? -1 : maxIndex + 1);
		}
		return res;
	}
	// 找到 不小于 target 的最小下标
	private int binarySearch(int[][] increase, int len, int col, int target) {
		int lt = 0, rt = len - 1, mid, res = len;
		while (lt <= rt) {
			mid = (lt + rt) >> 1;
			if (increase[mid][col] >= target) { // 靠左找
				rt = mid - 1;
				res = mid;
			} else // increase[mid][col] < target
				lt = mid + 1;
		}
		return res;
	}
}
```
