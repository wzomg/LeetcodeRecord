# 416.分割等和子集
题目链接：[传送门](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

## 题目描述：
给定一个**只包含正整数的非空**数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

**注意**:

- 每个数组中的元素不会超过`100`；
- 数组的大小不会超过`200`；

**示例**:

- 输入：`[1, 5, 11, 5]`
- 输出: `true`
- 解释: 数组可以分割成`[1, 5, 5]`和`[11]`。

## 解决方案：
- 时间复杂度：$O(n × W)$，W为所有物品的总数的一半
- 空间复杂度：$O(n × W)$
- 思路：每个元素看成每件物品，每个物品只有一件，将所有物品的重量相加sum后取一半sum/2作为背包的容量，那么问题转化成从n件物品中挑选出若干件物品，使得其总重量不超过sum/2时有最大价值（最大重量），即典型的01背包。

## AC代码1：
```java
class Solution {
	public boolean canPartition(int[] nums) {
		int len = nums.length, sum = 0, half;
		for (int i = 0; i < len; i++)
			sum += nums[i];
		if ((sum & 1) == 1) // 剪枝
			return false;
		half = sum >> 1;
		int[] dp = new int[half + 1];
		for (int i = 0; i < len; i++) {
			for (int j = half; j >= nums[i]; j--)
				dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
		}
		boolean flag = false;
		if ((dp[half] << 1) == sum)
			flag = true;
		return flag;
	}
}
```