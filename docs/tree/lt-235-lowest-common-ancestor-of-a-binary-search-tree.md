# 235.二叉搜索树的最近公共祖先
题目链接：[传送门](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

## 题目描述：
给定一个二叉搜索树，找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树`T`的两个结点`p`、`q`，最近公共祖先表示为一个结点`x`，满足`x`是`p`、`q`的祖先且`x`的深度尽可能大（一个节点也可以是它自己的祖先）。”

**说明**：

- 所有节点的值都是唯一的。
- p、q 为不同节点且均存在于给定的二叉搜索树中。

例如，给定如下二叉搜索树: `root = [6,2,8,0,4,7,9,null,null,3,5]`

![](../_media/binarysearchtree_improved.png)

**示例**:

- 输入：`root = [6,2,8,0,4,7,9,null,null,3,5]`, `p = 2`, `q = 8`
- 输出：`6` 
- 解释：节点2和节点8的最近公共祖先是6。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：根据二叉搜索树的特点，左子树所有节点的值都小于父节点，右子树所有节点的值都大于父节点。分3种情况讨论即可，具体看注释！

## AC代码：
```java
class Solution {
	public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
		if (root == null) // 终止条件
			return null;
		// 若都小于当前父节点的值，则继续递归左子树
		if (p.val < root.val && q.val < root.val)
			return lowestCommonAncestor(root.left, p, q);
		// 若都大于当前节点的值，则继续递归右子树
		if (p.val > root.val && q.val > root.val)
			return lowestCommonAncestor(root.right, p, q);
		// 若有一个与父节点值相等，或者一左一右，那么当前节点肯定是LCA
		return root;
	}
}
```