# 1014.最佳观光组合
题目链接：[传送门](https://leetcode-cn.com/problems/best-sightseeing-pair/)

## 题目描述：
给定正整数数组`A`，`A[i]`表示第`i`个观光景点的评分，并且两个景点`i`和`j`之间的距离为`j - i`。

一对景点（`i < j`）组成的观光组合的得分为（`A[i] + A[j] + i - j`）：景点的评分之和减去它们两者之间的距离。

返回一对观光景点能取得的最高分。

**提示**：

- $2 \leq$ `A.length` $ \leq 50000$
- $1 \leq$ `A[i]` $\leq 1000$


**示例**：

- 输入：`[8,1,5,2,6]`
- 输出：`11`
- 解释：`i = 0`, `j = 2`, `A[i] + A[j] + i - j = 8 + 5 + 0 - 2 = 11`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：简单思维。将`A[i] + A[j] + i - j`进行变换得`(A[i] + i) + (A[j] - j)`，因为`i < j`，所以应从后往前遍历，同时维护该表达式的最大值即可。

## AC代码：
```java
class Solution {
	public int maxScoreSightseeingPair(int[] A) {
		int len, res = 0, maxV;
		if (A == null || (len = A.length) == 0)
			return 0;
		maxV = A[len - 1] - len + 1;
		for (int i = len - 2; i >= 0; i--) {
			res = Math.max(A[i] + i + maxV, res);
			maxV = Math.max(maxV, A[i] - i);
		}
		return res;
	}
}
```