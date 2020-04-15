# 236.二叉树的最近公共祖先
题目链接：[传送门](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

## 题目描述：
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树`T`的两个结点`p`、`q`，最近公共祖先表示为一个结点`x`，满足`x`是`p`、`q`的祖先且`x`的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树: `root = [3,5,1,6,2,0,8,null,null,7,4]`

![binarytree.png](../_media/binarytree.png)

**示例**：

- 输入：`root = [3,5,1,6,2,0,8,null,null,7,4]`，`p = 5`，`q = 1`
- 输出: 3
- 解释: 节点 5 和节点 1 的最近公共祖先是节点 3。

## 解决方案：
- 时间复杂度：$O(n)$，`n`为树中的节点个数。
- 空间复杂度：$O(m)$，`m`为树的高度。
- 思路：站在某个节点上往下看，先序遍历左右子树，若左子树和右子树的返回值都不为空，那么当前父节点就是其最近公共祖先；若左子树不为空，则返回右子树递归遍历返回的节点，否则返回左子树递归遍历返回的节点。因为是回溯，所有节点的值都是唯一的，`p`、`q`为不同节点且均存在于给定的二叉树中，所以若返回的左右子树节点都不为空，则返回当前节点`root`（`LCA`）；若这个`root`是根的左子节点，则右子树递归遍历会返回空，即`LCA`就是那个`root`！

## AC代码：
```java
class Solution {
	public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
		if (root == null || root == p || root == q)
			return root;
		TreeNode lt = lowestCommonAncestor(root.left, p, q);
		TreeNode rt = lowestCommonAncestor(root.right, p, q);
		if (lt != null && rt != null)
			return root;
		return lt == null ? rt : lt;
	}
}
```