# 530.二叉搜索树的最小绝对差
题目链接：[传送门](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)

## 题目描述：
给你一棵所有节点为非负值的二叉搜索树，请你计算树中任意两节点的差的绝对值的最小值。

**提示**：树中至少有`2`个节点。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：中序遍历。二叉搜索树的最小绝对差一定是中序遍历过程中相邻元素差值的绝对值的最小值！

## AC代码：
```java
class Solution {
	private int pre, res;
	private boolean flag;
	public int getMinimumDifference(TreeNode root) {
		res = Integer.MAX_VALUE;
		flag = false;
		dfs(root);
		return res;
	}
	private void dfs(TreeNode root) {
		if (root == null)
			return;
		dfs(root.left);
		if (!flag)
			flag = true;
		else
			res = Math.min(Math.abs(root.val - pre), res);
		pre = root.val;
		dfs(root.right);
	}
}
```