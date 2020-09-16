# 215.数组中的第K个最大元素
题目链接：[传送门](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

## 题目描述：
在未排序的数组中找到第`k`个最大的元素。请注意，你需要找的是数组排序后的第`k`个最大的元素，而不是第`k`个不同的元素。

**说明**：你可以假设`k`总是有效的，且 $1 \leq k \leq $ 数组的长度。

## 解决方案：
- 时间复杂度：$O(n)$，最坏为 $O(n^2)$（给定初始状态为升序排）
- 空间复杂度：$O(log^n)$，最坏为 $O(n)$（给定初始状态为升序排）
- 思路：快速排序递归找某个part中第`k'`大！

## AC代码：
```java
class Solution {
	public int findKthLargest(int[] nums, int k) {
		int len;
		if (nums == null || (len = nums.length) == 0)
			return -1;
		search_kth_element(nums, 0, len - 1, k);
		return nums[k - 1];
	}
	private void search_kth_element(int[] nums, int low, int high, int k) {
		if (low >= high)
			return;
        // 注意：若将低位low作为枢纽，则应先从右往左遍历，否则会覆盖右边待排的元素
		int lt = low, rt = high, key = nums[low]; 
		while (lt < rt) {
			while (lt < rt && nums[rt] <= key)
				rt--;
            // 大于标杆的放左边
			nums[lt] = nums[rt];
			while (lt < rt && nums[lt] >= key)
				lt++;
            // 小于标杆的放右边
			nums[rt] = nums[lt]; 
		}
        // 放置标杆
		nums[lt] = key; 
		// 若当前key的位置lt正好是区间[low, lt]的第k'个，也就是全局[0,n-1]中的第k大值，表明已经排好目标元素，则直接返回
		if (lt - low + 1 == k)
			return;
		// 若当前key所在的左区间[low,lt]中的元素个数小于k'，说明全局[0,n-1]中的第k大值在key的右区间[lt+1,high]中，
		// 则往右区间排序，且下个区间只需找到该区间的第k'- (lt - low + 1)个位置的元素即可
		if (lt - low + 1 < k)
			search_kth_element(nums, lt + 1, high, k - (lt - low + 1));
		else // 否则说明全局[0,n-1]中的第k大值在key的左区间[low,rt - 1]，那么k'不变，继续在该子区间中排第k'个元素即可！
			search_kth_element(nums, low, rt - 1, k);
	}
}
```