# 1095.山脉数组中查找目标值
题目链接：[传送门](https://leetcode-cn.com/problems/find-in-mountain-array/)

## 题目描述：
（这是一个**交互式问题**）

给你一个 山脉数组`mountainArr`，请你返回能够使得`mountainArr.get(index)`**等于**`target`**最小**的下标`index`值。

如果不存在这样的下标`index`，就请返回`-1`。

何为山脉数组？如果数组`A`是一个山脉数组的话，那它满足如下条件：

首先，`A.length` $ \geq 3 $

其次，在 $0 < i <$ `A.length - 1` 条件下，存在`i`使得：

- `A[0] < A[1] < ... A[i-1] < A[i]`
- `A[i] > A[i+1] > ... > A[A.length - 1]`

你将**不能直接访问该山脉数组**，必须通过`MountainArray`接口来获取数据：

- `MountainArray.get(k)`：会返回数组中索引为`k`的元素（下标从`0`开始）
- `MountainArray.length()`：会返回该数组的长度

**注意**：

对`MountainArray.get`发起超过`100`次调用的提交将被视为错误答案。此外，任何试图规避判题系统的解决方案都将会导致比赛资格被取消。

**提示**：

- $3 \leq $ `mountain_arr.length()` $ \leq 10000 $
- $0 \leq $ `target` $ \leq 10^9 $
- $0 \leq $ `mountain_arr.get(index)` $\leq 10^9$

**示例**：

- 输入：`array = [1,2,3,4,5,3,1]`, `target = 3`
- 输出：`2`
- 解释：`3`在数组中出现了两次，下标分别为`2`和`5`，我们返回最小的下标`2`。

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(1)$
- 思路：题目中数组长度给的是1w，又因为访问接口次数不能超过100次，故考虑二分做法。先找出峰值的下标，再在左半部分中找目标值的最小下标，若其返回-1，则返回右半部分查找的最小下标即可！

## AC代码：
```java
class Solution {
	public int findInMountainArray(int target, MountainArray mountainArr) {
		int len = mountainArr.length();
		int peekIndex = findPeekIndex(0, len - 1, mountainArr);
		int res = findTargetIndex(0, peekIndex, target, mountainArr, true); // 升序找
		if (res == -1)
			return findTargetIndex(peekIndex + 1, len - 1, target, mountainArr, false); // 降序找
		return res;
	}
	// 查找目标值出现的最小下标
	private int findTargetIndex(int left, int right, int target, MountainArray mountainArr, boolean flag) {
		int lt = left, rt = right, mid, num, res = -1;
		while (lt <= rt) {
			mid = (lt + rt) >> 1;
			num = mountainArr.get(mid);
			if (flag) { // 升序数组，靠左边找
				if (target <= num) {
					rt = mid - 1;
					if (target == num)
						res = mid;
				} else // target > num
					lt = mid + 1;
			} else { // 降序数组，靠左边找
				if (target >= num) {
					rt = mid - 1;
					if (target == num)
						res = mid;
				} else
					lt = mid + 1;
			}
		}
		return res;
	}
	// 查找峰值
	private int findPeekIndex(int left, int right, MountainArray mountainArr) {
		int lt = left, rt = right, mid;
		while (lt < rt) { // 当 lt == rt 时退出循环，否则循环内至少有2个元素
			mid = lt + (rt - lt) / 2; // 下取整，当有2个元素时，取左边的元素
			if (mountainArr.get(mid) < mountainArr.get(mid + 1)) // 在左半段，定位右半段
				lt = mid + 1;
			else { // 在右半段，定位左半段
				rt = mid;
			}
		}
		return lt;
	}
}
```