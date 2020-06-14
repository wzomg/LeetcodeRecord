# 1300.转变数组后最接近目标值的数组和
题目链接：[传送门](https://leetcode-cn.com/problems/sum-of-mutated-array-closest-to-target/)

## 题目描述：
给你一个整数数组`arr`和一个目标值`target`，请你返回一个整数`value`，使得将数组中所有大于`value`的值变成`value`后，数组的和最接近`target`（最接近表示两者之差的绝对值最小）。

如果有多种使得和最接近`target`的方案，请你返回这些整数中的最小值。

请注意，答案不一定是`arr`中的数字。


**提示**：
- $1 \leq$ `arr.length` $ \leq 10^4$
- $1 \leq$ `arr[i], target` $\leq 10^5$

## 解决方案：
- 时间复杂度：$O(N \times log^N)$
- 空间复杂度：$O(N)$
- 思路：排序+二分。先将数组进行排序，因为数组元素最大值为 $10^5$，所以采用二分法：在区间 $[0, max(arr[i])]$ 中枚举 `value`，假设不小于 `value` 的下标为 `idx`，则每次需要计算一下 `sum[idx - 1] + (len - idx) * value` 与 `target` 之差，取其最小差值时的 `value` 即可。另外一种思路是对前面做法的优化，因为数组和呈现严格单调，所以可以在区间 $[0, max(arr[i])]$ 中进行二分查找，找出一个不小于且最接近 `target` 时 `value`的取值，最后判断一下是 `value - 1` 还是 `value`即可。注意：记录答案的变量：`res`要初始化为`max(arr[i])`。

## AC代码1：
```java
class Solution {
	public int findBestValue(int[] arr, int target) {
		int len, res = 0, diff = Integer.MAX_VALUE, curSum;
		if (arr == null || (len = arr.length) == 0)
			return 0;
		Arrays.sort(arr);
		int[] sum = new int[len];
		for (int i = 0; i < len; i++)
			sum[i] = ((i == 0) ? 0 : sum[i - 1]) + arr[i];
		for (int i = 0, rt = arr[len - 1], idx; i <= rt; i++) {
			idx = lower_bound(arr, i, len);
			// 肯定能找得到不小于元素i的正确下标，不会出现越界的情况 
			curSum = (idx == 0 ? 0 : sum[idx - 1]) + (len - idx) * i;
			if (Math.abs(curSum - target) < diff) {
				res = i;
				diff = Math.abs(curSum - target);
			}
		}
		return res;
	}
	// 找到第1个不小于 target 元素的下标
	private int lower_bound(int[] arr, int target, int len) {
		int lt = 0, rt = len - 1, res = len, mid;
		while (lt <= rt) {
			mid = (lt + rt) >> 1;
			if (arr[mid] >= target) {
				rt = mid - 1;
				res = mid;
			} else
				lt = mid + 1;
		}
		return res;
	}
}
```

## AC代码2：消除常数。
```java
class Solution {
	public int findBestValue(int[] arr, int target) {
		int len, res, curSum, mid, idx;
		if (arr == null || (len = arr.length) == 0)
			return 0;
		Arrays.sort(arr);
		int[] sum = new int[len];
		for (int i = 0; i < len; i++)
			sum[i] = ((i == 0) ? 0 : sum[i - 1]) + arr[i];
		int lt = 0, rt = arr[len - 1];
        // 注意：初始化为 max(arr[i])
		res = arr[len - 1]; 
		while (lt <= rt) {
			mid = (lt + rt) >> 1;
			idx = lower_bound(arr, mid, len);
			curSum = (idx == 0 ? 0 : sum[idx - 1]) + (len - idx) * mid;
			// System.out.println(idx + " " + mid + " " + curSum);
            // mid 太大，缩小上界
			if (curSum >= target) {
				res = mid;
				rt = mid - 1;
			} else // mid 太小，扩大下界
				lt = mid + 1;
		}
		int sum1 = getTotal(arr, res, len), sum2 = getTotal(arr, res - 1, len);
		// System.out.println(sum1 + " " + sum2);
        // 若 (sum2)' <= (sum1)'，则优先取 res - 1
		return Math.abs(sum1 - target) < Math.abs(sum2 - target) ? res : res - 1; 
	}
    // 计算数组元素之和
	private int getTotal(int[] arr, int value, int len) {
		int res = 0;
		for (int i = 0; i < len; i++)
			res += Math.min(arr[i], value);
		return res;
	}
	// 找到第1个不小于 target 元素的下标
	private int lower_bound(int[] arr, int target, int len) {
		int lt = 0, rt = len - 1, res = len, mid;
		while (lt <= rt) {
			mid = (lt + rt) >> 1;
			if (arr[mid] >= target) {
				rt = mid - 1;
				res = mid;
			} else
				lt = mid + 1;
		}
		return res;
	}
}
```