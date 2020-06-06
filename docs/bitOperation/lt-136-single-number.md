# 136.只出现一次的数字
题目链接：[传送门](https://leetcode-cn.com/problems/single-number/)

## 题目描述：
给定一个**非空**整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

**说明**：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

**示例**:

- 输入: `[4,1,2,1,2]`
- 输出: `4`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：异或思想，将数组元素全部进行异或即可！

## AC代码：
```java
class Solution {
	public int singleNumber(int[] nums) {
		int res = 0;
		for (int i = 0, len = nums.length; i < len; i++)
			res ^= nums[i];
		return res;
	}
}
```