# 面试题4.二维数组中的查找
题目链接：[传送门](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

## 题目描述：
在一个 $n \times m$ 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

**限制**：

- $0 \leq n \leq 1000$
- $0 \leq m \leq 1000$

**示例**：现有矩阵`matrix`如下：

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

- 给定`target = 5`，返回`true`。
- 给定`target = 20`，返回`false`。

## 解决方案：
- 时间复杂度：$O(n \times m)$
- 空间复杂度：$O(1)$
- 思路：双指针。从右上角开始往左下角依次遍历比较即可！

## AC代码：
```java
class Solution {
	public boolean findNumberIn2DArray(int[][] matrix, int target) {
		int m, n, i, j;
		if (matrix == null || (m = matrix.length) == 0 || (n = matrix[0].length) == 0)
			return false;
		i = 0;
		j = n - 1;
		while (i < m && j >= 0) {
			if (matrix[i][j] == target)
				return true;
			else if (matrix[i][j] < target)
				i++;
			else
				j--;
		}
		return false;
	}
}
```