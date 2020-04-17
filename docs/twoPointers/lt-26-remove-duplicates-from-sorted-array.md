# 26.删除排序数组中的重复项
题目链接：[传送门](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

## 题目描述：
给定一个排序数组，你需要在**原地**删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在**原地**修改输入数组 并在使用`O(1)`额外空间的条件下完成。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：双指针。每次不相等就将`i`赋值给`++rt`即可！

## AC代码：
```java
class Solution {
	public int removeDuplicates(int[] nums) {
		int rt = 0, len;
		if (nums == null || (len = nums.length) == 0)
			return 0;
		for (int i = 1; i < len; i++) {
			if (nums[rt] != nums[i]) {
				nums[++rt] = nums[i];
			}
		}
		return rt + 1;
	}
}
```
