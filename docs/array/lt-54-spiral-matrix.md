# 54.螺旋矩阵
题目链接：[传送门](https://leetcode-cn.com/problems/spiral-matrix/)

## 题目描述：
给定一个包含 $m \times n$ 个元素的矩阵（`m`行,`n`列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

**示例**:

- 输入：

```
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
```

- 输出: `[1,2,3,6,9,8,7,4,5]`

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n)$
- 思路：简单模拟即可。

## AC代码：
```java
class Solution {
	public List<Integer> spiralOrder(int[][] matrix) {
		List<Integer> res = new ArrayList<Integer>();
		int n, m;
		if (matrix == null || (n = matrix.length) == 0 || (m = matrix[0].length) == 0)
			return res;
		int i = 0, j = 0, cnt = 0, tot = n * m, deta = 0;
		while (cnt < tot) {
			while (cnt < tot && j < m - deta) {
				res.add(matrix[i][j++]);
				cnt++;
			}
			j--;
			i++;
			while (cnt < tot && i < n - deta) {
				res.add(matrix[i++][j]);
				cnt++;
			}
			i--;
			j--;
			while (cnt < tot && j >= deta) {
				res.add(matrix[i][j--]);
				cnt++;
			}
			i--;
			j++;
			deta++;
			while (cnt < tot && i >= deta) {
				res.add(matrix[i--][j]);
				cnt++;
			}
			i++;
			j++;
		}
		return res;
	}
}
```