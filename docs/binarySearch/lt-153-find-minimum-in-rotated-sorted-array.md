# 153.寻找旋转排序数组中的最小值
题目链接：[传送门](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

## 题目描述：
假设按照升序排序的数组在预先未知的某个点上进行了旋转。

（例如，数组`[0,1,2,4,5,6,7]`可能变为`[4,5,6,7,0,1,2]`）。

请找出其中最小的元素。

你可以假设数组中不存在重复元素。

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(1)$
- 思路：二分查找。每次二分成`2`个子数组，因为数组中不存在重复元素，所以其中一个子数组一定是单调递增的。抓住这个条件，先判断右半部分，再判断左半部分，详解看代码。

## AC代码：
```java
class Solution {
	public int findMin(int[] nums) {
		int len, lt = 0, rt, mid;
		if (nums == null || (len = nums.length) == 0)
			return -1;
		rt = len - 1;
		// 只剩下一个元素无需比较，不需要等号，剩下一个元素肯定是最小值
		while (lt < rt) {
			mid = (lt + rt) >> 1;
			// 其中一部分肯定是升序的，先判断右半部分
			if (nums[mid] > nums[rt]) // 最小值肯定在后半部分
				lt = mid + 1;
			else // nums[mid] < nums[rt]，最小值在[0,mid]中。注意：mid可能为最小值，下次查找的右边界为mid，这里跟循环条件不加等号有关。
				rt = mid;
		}
		return nums[lt];
	}
}
```