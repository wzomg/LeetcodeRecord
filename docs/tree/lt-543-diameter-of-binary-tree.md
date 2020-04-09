# 543.二叉树的直径
题目链接：[传送门](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

## 题目描述：
给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

## 示例:
给定二叉树，返回`3`，它的长度是路径`[4,2,1,3]`或者`[5,2,1,3]`。
```
          1
         / \
        2   3
       / \     
      4   5    
```
注意：两结点之间的路径长度是以它们之间边的数目表示。

## 解决方案：
- 时间复杂度：$O(n)$，`n`为树中的节点数。
- 空间复杂度：$O(m)$，`m`为树的高度。
- 思路：站在父节点往下看，取左右子树各自的最大深度之和做一个全局取max，因为离根节点越近的非叶子节点可能有更优的答案！

## AC代码：
```java
class Solution {
	private int res;
	public int diameterOfBinaryTree(TreeNode root) {
		res = 0;
		dfs(root);
		return res;
	}
	private int dfs(TreeNode root) {
		if (root == null) // 终止条件
			return 0;
		int cntL = dfs(root.left); // 找到左子树的最大高度
		int cntR = dfs(root.right); // 找到右子树的最大高度
		res = Math.max(res, cntL + cntR); // 当前节点作为父亲节点的左右子树中各自的最大高度相加和全局取个max
		return Math.max(cntL, cntR) + 1; // 返回当前节点的最大高度
	}
}
```