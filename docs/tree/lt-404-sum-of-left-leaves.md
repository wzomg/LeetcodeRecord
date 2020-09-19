# 404.左叶子之和
题目链接：[传送门](https://leetcode-cn.com/problems/sum-of-left-leaves/)

## 题目描述：
计算给定二叉树的所有左叶子之和。

**示例**：

```
    3
   / \
  9  20
    /  \
   15   7
```

在这个二叉树中，有两个左叶子，分别是`9`和`15`，所以返回`24`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：先序遍历，每次站在父节点，若其左子节点是叶子节点，则累加左子节点的值即可。

## AC代码：
```java
class Solution {
	private int res;
	public int sumOfLeftLeaves(TreeNode root) {
		res = 0;
		dfs(root);
		return res;
	}
	private void dfs(TreeNode root) {
		if (root == null)
			return;
		if (root.left != null && root.left.left == null && root.left.right == null)
			res += root.left.val;
		dfs(root.left);
		dfs(root.right);
	}
}
```