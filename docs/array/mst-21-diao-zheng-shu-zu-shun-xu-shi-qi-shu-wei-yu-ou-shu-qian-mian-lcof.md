# 面试题21.调整数组顺序使奇数位于偶数前面
题目链接：[传送门](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

## 题目描述：
输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

**提示**：

- $1 \leq$ `nums.length` $\leq 50000$
- $1 \leq$ `nums[i]` $\leq 10000$

**示例**：

- 输入：`nums = [1,2,3,4]`
- 输出：`[1,3,2,4]` 
- 注：`[3,1,2,4]`也是正确的答案之一。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：双指针。若当前数`(nums[lt])`是奇数，则直接`lt++`；否则与`rt`交换，并且`rt--`即可。

## AC代码：
```java
class Solution {
	public int[] exchange(int[] nums) {
		int lt = 0, len = nums.length, rt = len - 1;
		while (lt < rt) {
			if ((nums[lt] & 1) == 1)
				lt++;
			else {
				swap(nums, lt, rt);
				rt--;
			}
		}
		return nums;
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