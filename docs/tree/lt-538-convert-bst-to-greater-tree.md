# 538.把二叉搜索树转换为累加树
题目链接：[传送门](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

## 题目描述：
给定一个二叉搜索树（`Binary Search Tree`），把它转换成为累加树（`Greater Tree`），使得每个节点的值是原来的节点值加上所有大于它的节点值之和。

**例如**：

- 输入：原始二叉搜索树：

```
              5
            /   \
           2     13
```

- 输出：转换为累加树：

```
             18
            /   \
          20     13
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：按“右中左”的方式，累加后缀节点和res，每次用res替换为当前节点root.val的值即可。

## AC代码：
```java
class Solution {
	private int res;
	public TreeNode convertBST(TreeNode root) {
		res = 0;
		dfs(root);
		return root;
	}
	private void dfs(TreeNode root) {
		if (root == null)
			return;
		dfs(root.right);
		// 先累加当前节点
		res += root.val;
		// 然后将当前节点的值替换为 res即可
		root.val = res;
		dfs(root.left);
	}
}
```