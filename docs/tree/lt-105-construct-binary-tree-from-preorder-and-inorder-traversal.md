# 105.从前序与中序遍历序列构造二叉树
题目链接：[传送门](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

## 题目描述：
根据一棵树的前序遍历与中序遍历构造二叉树。

**注意**：你可以假设树中没有重复的元素。

例如，给出前序遍历：`preorder = [3,9,20,15,7]`，中序遍历：`inorder = [9,3,15,20,7]`。

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

## 解决方案：
- 时间复杂度：$O(n)$，n为数组长度
- 空间复杂度：$O(m)$，m为二叉树的高度
- 思路：根据前序遍历的特点，第一个肯定是当前父节点，然后在中序遍历中找到其位置，左边为其左子树，右边为其右子树，递归构造二叉树即可。

## AC代码：
```java
class Solution {
	public TreeNode buildTree(int[] preorder, int[] inorder) {
		if (preorder == null || inorder == null)
			return null;
		return dfs(0, preorder.length, preorder, 0, inorder.length, inorder);
	}
	private TreeNode dfs(int st1, int len1, int[] pre, int st2, int len2, int[] ino) {
		if (len1 < 1)
			return null;
		TreeNode root = new TreeNode(pre[st1]); // 当前父节点
		int l2 = 0;
		for (int i = 0; i < len2; i++) { // 从中序遍历中找到父节点
			if (ino[st2 + i] == pre[st1])
				break;
			l2++;
		}
		root.left = dfs(st1 + 1, l2, pre, st2, l2, ino); // 左边成为左子树
		root.right = dfs(st1 + l2 + 1, len1 - 1 - l2, pre, st2 + l2 + 1, len2 - l2 - 1, ino); // 右边成为右子树
		return root; // 返回当前父节点
	}
}
```