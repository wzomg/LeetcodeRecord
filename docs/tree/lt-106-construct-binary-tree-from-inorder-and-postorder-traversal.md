# 106.从中序与后序遍历序列构造二叉树
题目链接：[传送门](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

## 题目描述：
根据一棵树的中序遍历与后序遍历构造二叉树。

**注意**：你可以假设树中没有重复的元素。

**例如**：给出

- 中序遍历：`inorder = [9,3,15,20,7]`
- 后序遍历：`postorder = [9,15,7,20,3]`

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：注意题目中说了没有重复的元素！根据后续遍历的特点：左右根，每个区间段的最后一个节点一定是当前子树的根节点，于是我们就可以在中序遍历中确定根节点的位置，将其一分为二，递归创建左右子树即可。

## AC代码：
```java
class Solution {
	public TreeNode buildTree(int[] inorder, int[] postorder) {
		int len;
		if (inorder == null || postorder == null || (len = inorder.length) == 0)
			return null;
		return dfs(inorder, 0, len, postorder, 0);
	}
	private TreeNode dfs(int[] in, int st1, int len, int[] po, int st2) {
		if (len < 1)
			return null;
		// 后续遍历序列的最后一个节点就是当前根节点
		TreeNode root = new TreeNode(po[st2 + len - 1]);
		System.out.println(root.val);
		int i = 0;
		// 找到中序遍历的根节点
		for (; i < len; i++)
			if (in[st1 + i] == po[st2 + len - 1])
				break;
		// 递归构建左右子树
		root.left = dfs(in, st1, i, po, st2);
		root.right = dfs(in, st1 + i + 1, len - (i + 1), po, st2 + i);
		return root;
	}
}
```