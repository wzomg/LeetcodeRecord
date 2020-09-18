# 124.二叉树中的最大路径和
题目链接：[传送门](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

## 题目描述：
给定一个**非空**二叉树，返回其最大路径和。

本题中，路径被定义为一条从树中任意节点出发，沿父节点-子节点连接，达到任意节点的序列。该路径**至少包含一个**节点，且不一定经过根节点。

**示例**：

- 输入：`[-10,9,20,null,null,15,7]`

```
   -10
   / \
  9  20
    /  \
   15   7
```
- 输出：`42`（15+20+7=42）

## 解决方案：
- 时间复杂度：$O(n)$，n为树中的节点数
- 空间复杂度：$O(m)$，m为树的高度
- 思路：先序遍历，每次都考虑当前节点所在的左支链（权值为非负数）连上右支链（权值为非负数）是否构成二叉树中的最大路径即可！

## AC代码：
```java
class Solution {
	private int res;
	public int maxPathSum(TreeNode root) {
		// 初始花为无穷小，若树中每个节点的值全是负数，则取其中绝对值最小即可
		res = Integer.MIN_VALUE;
		dfs(root);
		return res;
	}
	private int dfs(TreeNode root) {
		if (root == null) // 若为空，则返回0
			return 0;
		// 若为负数就取0，0表示不包含该子树
		int lt = Math.max(0, dfs(root.left));
		// 若为负数就取0，0表示不包含该子树
		int rt = Math.max(0, dfs(root.right));
		// 此处包含了4种情况
		// 考虑最长路径可能包括当前节点的值，也可能包括左右子树路径值
		res = Math.max(res, root.val + lt + rt);
		// 取子树某支链上的最大值加上当前节点的值
		return Math.max(lt, rt) + root.val;
	}
}
```