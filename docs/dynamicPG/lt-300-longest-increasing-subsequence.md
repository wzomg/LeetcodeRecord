# 300.最长上升子序列
题目链接：[传送门](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

## 题目描述：
给定一个无序的整数数组，找到其中最长上升子序列的长度。

**说明**:

可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。

你算法的时间复杂度应该为 $O(n^2)$ 。

**进阶**: 你能将算法的时间复杂度降低到 $O(n \times log^n)$ 吗?

## 解决方案：
- 时间复杂度：$O(n \times log^n)$
- 空间复杂度：$O(n)$
- 思路：采用二分法每次更新最小序列，最终最小序列的长度（其最终的序列不一定是正确的`LIS`，只是某个过程中有这个最长的序列，其长度就是最终小于`INF`的元素个数）就是最长上升子序列长度。

## AC代码：
```java
class Solution {
	public int lengthOfLIS(int[] nums) {
		int len = 0;
		if (nums == null || (len = nums.length) < 2)
			return len;
		int[] dp = new int[len];
		Arrays.fill(dp, 0x3f3f3f3f);
		for (int i = 0; i < len; i++)
			dp[lower_bound(dp, nums[i], len)] = nums[i];
		return lower_bound(dp, 0x3f3f3f3f, len);
	}
	// 二分找dp数组中第一个不小于x的下标
	// dp数组是一个单调不减数组
	private int lower_bound(int[] dp, int x, int len) {
		int lt = 0, rt = len - 1, mid, res = len; // 找不到符合条件默认返回数组长度len，实际上一定找得到，只要给的数组元素都不大于 INF 
		while (lt <= rt) {
			mid = (lt + rt) >> 1;
			// 找更小的 mid 
			if (dp[mid] >= x) {
				rt = mid - 1;
				res = mid;
			} else
				lt = mid + 1;
		}
		return res;
	}
}
```