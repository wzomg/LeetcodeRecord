# 226.翻转二叉树
题目链接：[传送门](https://leetcode-cn.com/problems/invert-binary-tree/)

## 题目描述：
翻转一棵二叉树。

**输入**：

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

**输出**：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

## 解决方案：
- 时间复杂度：$O(n)$，$n$ 为树中节点的个数
- 空间复杂度：$O(n)$，（最坏），$n$ 为树中节点的个数
- 思路：后序遍历。简单的一个想法：站在根节点位置往下看，先递归遍历出左右儿子节点，再交换一下其两者的位置即可！

## AC代码：
```java
class Solution {
	public TreeNode invertTree(TreeNode root) {
		if (root == null)
			return null;
        // 后序遍历
		TreeNode rightNode = invertTree(root.left); 
		TreeNode leftNode = invertTree(root.right);
		root.left = leftNode;
		root.right = rightNode;
		return root;
	}
}
```