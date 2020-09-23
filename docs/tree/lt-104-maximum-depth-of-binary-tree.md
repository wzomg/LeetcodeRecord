# 104.二叉树的最大深度
题目链接：[传送门](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

## 题目描述：
给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明**：叶子节点是指没有子节点的节点。

**示例**：给定二叉树`[3,9,20,null,null,15,7]`，返回它的最大深度`3`。

```
    3
   / \
  9  20
    /  \
   15   7
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：简单dfs。站在当前节点往下看，递归得到左右子树的深度，返回两者的最大值+1（当前这一层）。

## AC代码：
```java
class Solution {
	public int maxDepth(TreeNode root) {
		if (root == null)
			return 0;
		int m = maxDepth(root.left);
		int n = maxDepth(root.right);
		return Math.max(m, n) + 1;
	}
}
```