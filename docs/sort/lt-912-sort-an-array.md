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
- 思路：**快速排序**实现。其思想就是：先找一个轴心元素`key`，小于`key`的元素放左边，大于`key`的元素放右边，最后当`lt==rt`时，就将`key`元素填坑，然后往左右两部分一直查找并摆放轴心元素即可！**堆排序**实现。其思想就是：从最后一个非叶子节点开始向上调整构建一个大顶堆，必须满足所有非叶子节点的值都不小于其2个子节点的值，然后每次将堆顶(下标`0`)与下标`j-1`上的元素进行交换，那么数组中较大值依次沉底，继续从堆顶往下调整成为一个大顶堆，循环操作直到剩下一个元素为止。

## AC代码1：快速排序递归法
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

## AC代码2：快速排序非递归法
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

## AC代码3：大顶堆实现
```java
class Solution {
	public int[] sortArray(int[] nums) {
		Heap_sort(nums, nums.length);
		return nums;
	}
	// 堆排序(大根堆实现升序排序)
	private void Heap_Adjust(int[] s, int cur, int len) {
		int tmp = s[cur];// 先取出当前元素cur
		for (int j = 2 * cur + 1; j < len; j = 2 * j + 1) {// 向下筛选
			if (j + 1 < len && s[j] < s[j + 1]) // 若左子节点的值小于右子节点，则j指向右子节点
				++j;
			if (tmp >= s[j]) // 若父节点的值大于子节点的值，则不用继续交换操作
				break;
			s[cur] = s[j]; // 否则将子节点j指向的较大值交给父节点cur
			cur = j; // 将交换下来的值继续进行与其子节点进行比较，此时 j 为原先那个较小值
		}
		s[cur] = tmp; // 把 tmp 放到正确的节点上
	}
	private void Heap_sort(int[] s, int len) {
		// 1、先构建一个大根堆
		for (int i = (len - 1) / 2; i >= 0; --i)
			Heap_Adjust(s, i, len);
		// 2、交换堆顶元素与末尾元素 + 调整堆结构
		for (int i = len - 1; i > 0; --i) {
			swap(s, i, 0); // 将堆顶元素换下来
			Heap_Adjust(s, 0, i);// 将[0,i-1]重新调整为大根堆
		}
	}
	private void swap(int[] s, int i, int j) {
		if (i == j || s[i] == s[j])
			return;
		s[i] ^= s[j];
		s[j] ^= s[i];
		s[i] ^= s[j];
	}
}
```