# 560.和为K的子数组
题目链接：[传送门](https://leetcode-cn.com/problems/subarray-sum-equals-k/)

## 题目描述：
给定一个整数数组和一个整数`k`，你需要找到该数组中和为`k`的连续的子数组的个数。

**说明**:

- 数组的长度为`[1, 20000]`。
- 数组中元素的范围是`[-1000, 1000]`，且整数`k`的范围是`[-1e7, 1e7]`。

**示例**:

- 输入：`nums = [1,1,1]`, `k = 2`
- 输出: `2`,`[1,1]`与`[1,1]`为两种不同的情况。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：前缀和思想。`sum(i-1) = sum(j) - k`，即`[i,j]`之间的前缀和之差刚好为`k`，注意初始化`cnt[0]=1`！

## AC代码：
```java
class Solution {
	public int subarraySum(int[] nums, int k) {
		int len, res = 0, sum = 0;
		if (nums == null || (len = nums.length) == 0)
			return res;
		Map<Integer, Integer> cnt = new HashMap<Integer, Integer>();
		// 键为0表示第一次遇到前缀和为k的连续子数组，此时需要累加1：cnt.get(0)=1
		// 表示某个下标i能延申到下标为-1的位置，也就是囊括了前面一整段。
		cnt.put(0, 1);
		for (int i = 0; i < len; i++) {
			sum += nums[i];
			if (cnt.containsKey(sum - k))
				res += cnt.get(sum - k);
			cnt.put(sum, cnt.getOrDefault(sum, 0) + 1);
		}
		return res;
	}
}
```
