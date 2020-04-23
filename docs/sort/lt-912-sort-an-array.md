# 912.排序数组
题目链接：[传送门](https://leetcode-cn.com/problems/sort-an-array/)

## 题目描述：
给你一个整数数组`nums`，请你将该数组升序排列。

**提示**：
- $1 \leq $`nums.length` $\leq 50000$
- $-50000 \leq $ `nums[i]` $\leq 50000$

## 解决方案：
- 时间复杂度：$O(n × log^n)$
- 空间复杂度：$O(n)$
- 思路：**快速排序**实现。其思想就是：先找一个轴心元素`key`，小于`key`的元素放左边，大于`key`的元素放右边，最后当`lt==rt`时，就将`key`元素填坑，然后往左右两部分一直查找并摆放轴心元素即可！

## AC代码1：递归法
```java
class Solution {
	public int[] sortArray(int[] nums) {
		quickSort(0, nums.length - 1, nums);
		return nums;
	}
	private void quickSort(int low, int high, int[] nums) {
		if (low >= high)
			return;
		int lt = low, rt = high, key = nums[lt];
		while (lt < rt) {
			while (lt < rt && nums[rt] >= key)
				rt--;
			nums[lt] = nums[rt];
			while (lt < rt && nums[lt] <= key)
				lt++;
			nums[rt] = nums[lt];
		}
		nums[lt] = key;
		quickSort(low, lt - 1, nums);
		quickSort(rt + 1, high, nums);
	}
}
```

## AC代码2：非递归法
```java
class Solution {
	public int[] sortArray(int[] nums) {
		int len, lt, rt, pivot;
		if (nums == null || (len = nums.length) < 2)
			return nums;
		Stack<Integer> stack = new Stack<Integer>();
		stack.push(0);
		stack.push(len - 1);
		while (!stack.isEmpty()) {
			rt = stack.pop();
			lt = stack.pop();
			// 找到轴心，继续往左右两部分找
			pivot = getPivot(lt, rt, nums);
			if (lt < pivot - 1) {
				stack.push(lt);
				stack.push(pivot - 1);
			}
			if (pivot + 1 < rt) {
				stack.push(pivot + 1);
				stack.push(rt);
			}
		}
		return nums;
	}
	// 找到轴心，即某个元素已排好序的位置
	private int getPivot(int low, int high, int[] nums) {
		int lt = low, rt = high, key = nums[low];
		while (lt < rt) {
			while (lt < rt && nums[rt] >= key)
				rt--;
			nums[lt] = nums[rt];
			while (lt < rt && nums[lt] <= key)
				lt++;
			nums[rt] = nums[lt];
		}
		nums[lt] = key;
		return lt;
	}
}
```