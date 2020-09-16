# 41.缺失的第一个正数
题目链接：[传送门](https://leetcode-cn.com/problems/first-missing-positive/)

## 题目描述：
给你一个未排序的整数数组，请你找出其中没有出现的最小的正整数。

**提示**：你的算法的时间复杂度应为 $ O(n) $，并且只能使用常数级别的额外空间。

**示例**:

- 输入: `[3,4,-1,1]`
- 输出: `2`

## 解决方案：
- 时间复杂度：$O(n)$，均摊复杂度。
- 空间复杂度：$O(1)$
- 思路：由题意分析可知要找的答案一定在`[1, len + 1]`中，考虑将数字`1`交换到下标`0`的位置上，数字`2`交换到下标`1`的位置上，依次类推。详解看代码注释！

## AC代码：
```java
class Solution {
	public int firstMissingPositive(int[] nums) {
		int len;
		if (nums == null || (len = nums.length) == 0)
			return 1;
		for (int i = 0; i < len; i++) {
			// 数组元素的范围必须是[1,len]，并且循环交换直到 nums[i]-1 下标上的元素已摆放好正确元素
			while (nums[i] >= 1 && nums[i] <= len && nums[nums[i] - 1] != nums[i]) {
				// 交换下标 i 和 下标 nums[i] - 1 的2个元素
				swap(nums, i, nums[i] - 1);
			}
		}
		for (int i = 0; i < len; i++) {
			if (nums[i] != i + 1) // 若出现下标i上的元素不等于i+1，则返回i+1
				return i + 1;
		}
		return len + 1; // 否则返回 len + 1
	}
	private void swap(int[] nums, int i, int j) {
		if (i == j || nums[i] == nums[j])
			return;
		nums[i] ^= nums[j];
		nums[j] ^= nums[i];
		nums[i] ^= nums[j];
	}
}
```