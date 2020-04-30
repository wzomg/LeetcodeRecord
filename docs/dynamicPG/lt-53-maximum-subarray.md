# 53.最大子序和
题目链接：[传送门](https://leetcode-cn.com/problems/maximum-subarray/)

## 题目描述：
给定一个整数数组`nums`，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**进阶**：

如果你已经实现复杂度为 $O(n)$ 的解法，尝试使用更为精妙的分治法求解。

**示例**：

- 输入: `[-2,1,-3,4,-1,2,1,-5,4]`,
- 输出: `6`
- 解释: 连续子数组 `[4,-1,2,1]` 的和最大，为`6`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：动态规划。要使得连续子串和最大，若数列的元素全是负数，则只能选择绝对值最小的负数；若包含正负数，则如何求解呢？考虑已求得连续子串的值为`nowSum>=0`，最大和为`maxVal`，现考虑是否加入`nums[i]`，当`nums[i] >= 0`时，显然要加入，即`sum+nums[i]>= nums[i] >= 0`（注意等号要加）；若`nums[i] < 0`，则当`nowSum + nums[i] >= 0 > nums[i]`，继续累加`nums[i]`（因为可能出现更大的值），即`nowSum +=nums[i]`；当 `nums[i] <= nowSum + nums[i] < 0`，显然只有当`nowSum = 0` 且 `nums[i] < 0` 时才成立。也就是合并为下面这种情况：当`nowSum<0`时，显然最多只能单独成一个元素，不能再继续添加了，其每次都会被更新为当前值即`nowSum = nums[i]`。综上所述，当`nowSum + nums[i] >= nums[i]`时`nowSum+=nums[i]`，否则`nowSum = nums[i]`。

## AC代码：
```java
class Solution {
	public int maxSubArray(int[] nums) {
		int res = 0, len, sum;
		if (nums == null || (len = nums.length) == 0)
			return 0;
		res = sum = nums[0]; // 先取出第一个元素
		for (int i = 1; i < len; i++) {
			if (sum + nums[i] < nums[i])
				sum = nums[i];
			else
				sum += nums[i];
			res = Math.max(res, sum);
		}
		return res;
	}
}
```
