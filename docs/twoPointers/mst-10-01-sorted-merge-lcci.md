# 面试题10.01.合并排序的数组
题目链接：[传送门](https://leetcode-cn.com/problems/sorted-merge-lcci/)

## 题目描述：
给定两个排序后的数组`A`和`B`，其中`A`的末端有足够的缓冲空间容纳`B`。 编写一个方法，将`B`合并入`A`并排序。

初始化`A`和`B`的元素数量分别为`m`和`n`。

**说明**：`A.length` $== n + m$

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：双指针逆序遍历填充即可！

## AC代码：
```java
class Solution {
	public void merge(int[] A, int m, int[] B, int n) {
		int cur = A.length - 1, i = m - 1, j = n - 1;
		while (i >= 0 && j >= 0) {
			if (A[i] > B[j])
				A[cur--] = A[i--];
			else
				A[cur--] = B[j--];
		}
		// 若j<0，则不用处理；若i<0，则A剩下的位置用B填满
		if (i < 0) {
			while (cur >= 0)
				A[cur--] = B[j--];
		}
	}
}
```