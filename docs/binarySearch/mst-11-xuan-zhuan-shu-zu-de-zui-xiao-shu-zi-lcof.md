# 面试题11.旋转数组的最小数字
题目链接：[传送门](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

## 题目描述：
把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组`[3,4,5,1,2]`为`[1,2,3,4,5]`的一个旋转，该数组的最小值为`1`。  

**示例**：

- 输入：`[3,4,5,1,2]`
- 输出：`1`

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(1)$
- 思路：贪心+二分！具体看代码注释。

## AC代码：
```java
class Solution {
	public int minArray(int[] numbers) {
		int len = numbers.length, lt = 0, rt = len - 1, mid;
		while (lt <= rt) {
			mid = (lt + rt) >> 1;
			// 题目说的是递增的，但其实排序后数组是非递减的
			// 第一轮就可以确定二分的方向
			if (numbers[mid] < numbers[rt]) // 在非递减序列内
				rt = mid; // mid 可能为最小值，所以不能直接将rt指向mid-1
			else if (numbers[mid] > numbers[rt]) // 但若中间数大于最右边的数，那么最小数肯定在mid右边，所以lt指向mid+1
				lt = mid + 1;
			else // 若两者值相同，只能让右指针向左移一个单位，贪心取最左边最小值
				rt--;
		}
		return numbers[lt];
	}
}
```