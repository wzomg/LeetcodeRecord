# 16.最接近的三数之和
题目链接：[传送门](https://leetcode-cn.com/problems/3sum-closest/)

## 题目描述：
给定一个包括`n`个整数的数组`nums`和 一个目标值`target`。找出`nums`中的三个整数，使得它们的和与`target`最接近。返回这三个数的和。假定每组输入只存在唯一答案。

**示例**：

例如，给定数组`nums = [-1，2，1，-4]`, 和`target = 1`。与`target`最接近的三个数的和为`2`。`(-1 + 2 + 1 = 2)`。

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(1)$
- 思路：双指针问题。先排序，然后从左往右依次遍历每个元素，当三数之和`sum`与`target`的绝对值之差有较小值时保存一下`sum`即可！

## AC代码：
```java
class Solution {
	public int threeSumClosest(int[] nums, int target) {
		int len, res, lt, rt, sum, positive;
		if (nums == null || (len = nums.length) == 0)
			return -1;
		Arrays.sort(nums);
		positive = res = Integer.MAX_VALUE;
		for (int i = 0; i < len - 2; i++) {
			lt = i + 1;
			rt = len - 1;
			while (lt < rt) {
				sum = nums[i] + nums[lt] + nums[rt];
				if (Math.abs(sum - target) < positive) {
					positive = Math.abs(sum - target);
					res = sum;
				}
				if (sum > target) // 让三者之和尽量靠近 target
					rt--;
				else
					lt++;
			}
		}
		return res;
	}
}
```
