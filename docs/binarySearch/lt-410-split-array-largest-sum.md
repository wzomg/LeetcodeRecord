# 410.分割数组的最大值
题目链接：[传送门](https://leetcode-cn.com/problems/split-array-largest-sum/)

## 题目描述：
给定一个非负整数数组和一个整数`m`，你需要将这个数组分成`m`个非空的连续子数组。设计一个算法使得这`m`个子数组各自和的最大值最小。

**注意**：数组长度`n`满足以下条件:

- $ 1 \leq n \leq 1000 $
- $ 1 \leq m \leq min(50, n) $

## 解决方案：
- 时间复杂度：$O(n × log^{\sum_{i=1}^{n} nums[i]})$
- 空间复杂度：$O(1)$
- 思路：典型的二分求最大值最小化。因为要求分割成子数组中各自和的最大值最小，所以我们只需在`[max(nums),sum(nums)]`中二分查找`mid`，判断其是否可以将整个数组划分成不超过`m`的子数组个数，并且不断夹逼最小化`mid`即可。注意：要用`long`类型，避免数据溢出！

## AC代码：
```java
class Solution {
	public int splitArray(int[] nums, int m) {
		int len;
		if (nums == null || (len = nums.length) == 0)
			return 0;
		long lt = 0, rt = 0, mid, res = 0;
		for (int i = 0; i < len; i++) {
			lt = Math.max(lt, nums[i]);
			rt += nums[i];
		}
		while (lt <= rt) {
			mid = (lt + rt) >> 1;
			if (check(nums, len, mid, m)) // cnt > m：下界太小了，分割的子数组太多，调大下界
				lt = mid + 1;
			else { // cnt <= m：上界比较大，调小上界
				rt = mid - 1;
				res = mid; // 同时记录 mid
			}
		}
		return (int) res;
	}
	private boolean check(int[] nums, int len, long limit, int m) {
		int cnt = 1; // 初始化为1
		long sum = nums[0]; // 第一个和也为 nums[0]
		for (int i = 1; i < len; i++) { // 从第二个元素开始遍历
			if (sum + nums[i] > limit) { // 划分第 cnt + 1 个子数组（新的子数组的第一个元素）
				cnt++;
				sum = nums[i];
			} else
				sum += nums[i];
			if (cnt > m) // 当划分的子数组个数超过m时，直接返回true
				return true;
		}
		return false; // 找到一种可能的分割方案
	}
}
```