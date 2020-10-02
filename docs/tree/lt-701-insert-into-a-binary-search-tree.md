# 701.二叉搜索树中的插入操作
题目链接：[传送门](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

## 题目描述：
给定二叉搜索树（`BST`）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据保证，新值和原始二叉搜索树中的任意节点值都不同。

注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回任意有效的结果。

**提示**：

- 给定的树上的节点数介于`0`和 $10^4$ 之间；
- 每个节点都有一个唯一整数值，取值范围从 0 到 $10^8$；
- $-10^8 \leq val \leq 10^8 $；
- 新值和原始二叉搜索树中的**任意节点值都不同**；

**例如**：给定二叉搜索树，和 插入的值：5

```
        4
       / \
      2   7
     / \
    1   3
```

你可以返回这个二叉搜索树:

```
         4
       /   \
      2     7
     / \   /
    1   3 5
```

或者这个树也是有效的：

```
         5
       /   \
      2     7
     / \   
    1   3
         \
          4
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：中序遍历。确定待插入节点值的遍历方向，选择一个满足条件且有空儿子节点插入即可。

## AC代码：
```java
class Solution {
	private boolean flag;
	public TreeNode insertIntoBST(TreeNode root, int val) {
		// 单独处理树为空的情况
		if (root == null) {
			root = new TreeNode(val);
			return root;
		}
		flag = false;
		dfs(root, val);
		return root;
	}
	private void dfs(TreeNode root, int val) {
		if (root == null || flag)
			return;
		// 若待插入节点的值小于当前根节点的值，则往左子树递归
		if (val < root.val)
			dfs(root.left, val);
		// 若是最后一个节点，则考虑插入其后
		if (root.val > val && root.left == null) {
			root.left = new TreeNode(val);
			flag = true;
			return;
		}
		if (root.val < val && root.right == null) {
			root.right = new TreeNode(val);
			flag = true;
			return;
		}
		// 若待插入节点的值大于当前根节点的值，则往右子树递归
		if (val > root.val)
			dfs(root.right, val);
	}
}
```