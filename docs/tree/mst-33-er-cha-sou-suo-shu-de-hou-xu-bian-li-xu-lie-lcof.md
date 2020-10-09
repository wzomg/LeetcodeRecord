# 面试题33.二叉搜索树的后序遍历序列
题目链接：[传送门](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

## 题目描述：
输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回`true`，否则返回`false`。假设输入的数组的任意两个数字都互不相同。

**提示**：数组长度 $ \leq 1000$

参考以下这颗二叉搜索树：

```
     5
    / \
   2   6
  / \
 1   3
```

**示例1**：

- 输入: `[1,6,3,2,5]`
- 输出: `false`

**示例2**：

- 输入: `[1,3,2,6,5]`
- 输出: `true`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$，m为树的高度
- 思路：每次取出当前区间的最后一个元素，从区间头开始遍历找到第一个大于rt的位置k，那么`[k,rt-1]`中所有元素的值都必须大于rt的值，并且把树分成左右子树，继续递归判断即可！

## AC代码：
```java
class Solution {
	public boolean verifyPostorder(int[] postorder) {
		int len;
		if (postorder == null || (len = postorder.length) == 0)
			return true; // 空树：ok
		return dfs(postorder, 0, len - 1);
	}
	private boolean dfs(int[] postorder, int lt, int rt) {
		if (rt - lt < 1) // 当前区间要比较的元素个数不超过1个则直接返回true
			return true;
		int k = lt;
		// 找到第一个大于rt的值
		while (k < rt && postorder[k] <= postorder[rt])
			k++;
		// 判断[k, rt)中所有元素是否都大于rt的值，若不是则直接返回false
		// 测试样例：1 3 2 7 8 6 5
		for (int i = k; i < rt; i++)
			if (postorder[i] < postorder[rt])
				return false;
		return dfs(postorder, lt, k - 1) && dfs(postorder, k, rt - 1);
	}
}
```