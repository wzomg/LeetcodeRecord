# 面试题51.数组中的逆序对
题目链接：[传送门](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

## 题目描述：
在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

**限制**：$0 \leq$ 数组长度 $\leq 50000$

**示例**：

- 输入: `[7,5,6,4]`
- 输出: `5`

## 解决方案：
- 时间复杂度：$O(n × log^n)$
- 空间复杂度：$O(n)$
- 思路：`cdq`分治，也算是归并排序，简单计数即可！

## AC代码：
```java
class Solution {
	private int len;
	private int[] tmp;
	public int reversePairs(int[] nums) {
		if (nums == null || (len = nums.length) == 0)
			return 0;
		tmp = new int[len]; // 辅助数组
		return dfs(0, len - 1, nums);
	}
	// 归并排序求逆序对
	private int dfs(int lt, int rt, int[] nums) {
		if (lt >= rt)
			return 0;
		int mid = (lt + rt) >> 1;
		int res = dfs(lt, mid, nums) + dfs(mid + 1, rt, nums);
		// 固定一个指针j，分成两半求逆序对的个数
		int i = lt, j = mid + 1, k = lt;
		while (j <= rt) {
			while (i <= mid && nums[i] <= nums[j]) // 循环填充比j小的i
				tmp[k++] = nums[i++];
			res += mid - i + 1; // 统计答案！
			tmp[k++] = nums[j++];
		}
		while (i <= mid) // 可能多余的i还没到达mid
			tmp[k++] = nums[i++];
		for (int h = lt; h <= rt; h++) // 重新拷贝复制到原数组
			nums[h] = tmp[h];
		return res;
	}
}
```