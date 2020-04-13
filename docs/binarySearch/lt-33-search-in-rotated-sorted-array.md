# 33.搜索旋转排序数组
题目链接：[传送门](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

## 题目描述：
假设按照升序排序的数组在预先未知的某个点上进行了旋转。

（例如，数组`[0,1,2,4,5,6,7]`可能变为`[4,5,6,7,0,1,2]`）。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回`-1`。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 $O(log^n)$ 级别。

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(1)$
- 思路：二分查找。每次二分成`2`个子数组，因为数组中不存在重复元素，所以其中一个子数组一定是单调递增的。抓住这个条件，在有序区间进行目标值判断即可，具体看代码注释。注意：若其中一部分只有1个元素，如`[3, 1]`，则第一次判断应该用`3`和`target`进行比较，这样就不会出错。

## AC代码：
```java
class Solution {
	public int search(int[] nums, int target) {
		int len, lt = 0, rt, mid;
		if (nums == null || (len = nums.length) == 0)
			return -1;
		rt = len - 1;
		while (lt <= rt) {
			mid = (lt + rt) >> 1;
			// 若找到则直接返回
			if (target == nums[mid])
				return mid;
			// 二分数组，肯定有一半是升序的，分成两种情况讨论：
			// 注意第一个判断要加等号，如当数组为[3, 1]，目标元素为1，第一次二分后左半部分只有一个元素：3，此时应该用3与target进行比较
			if (nums[lt] <= nums[mid]) { // 有序数组
				if (nums[lt] <= target && target <= nums[mid])
					rt = mid - 1;
				else
					lt = mid + 1;
			} else { 
				if (nums[mid] <= target && target <= nums[rt])
					lt = mid + 1;
				else
					rt = mid - 1;
			}
		}
		return -1;
	}
}
```