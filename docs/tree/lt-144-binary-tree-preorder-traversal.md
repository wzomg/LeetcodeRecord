# 144.二叉树的前序遍历
题目链接：[传送门](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

## 题目描述：
给定一个二叉树，返回它的**前序**遍历。

**进阶**: 递归算法很简单，你可以通过迭代算法完成吗？

**示例**:

- 输入: `[1,null,2,3]`

```
   1
    \
     2
    /
   3 
```

- 输出: `[1,2,3]`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：递归法：根→左→右。非递归法：按照先序遍历的方式每次将当前节点入栈，然后一直往左子节点走，直到为空，再弹出栈顶元素进入右子节点继续同样的操作即可。

## AC代码1：递归法。
```java
class Solution {
	public List<Integer> preorderTraversal(TreeNode root) {
		List<Integer> res = new ArrayList<>();
		dfs(root, res);
		return res;
	}
	private void dfs(TreeNode root, List<Integer> cur) {
		if (root == null)
			return;
		cur.add(root.val);
		dfs(root.left, cur);
		dfs(root.right, cur);
	}
}
```

## AC代码2：非递归法。
```java
class Solution {
	public List<Integer> preorderTraversal(TreeNode root) {
		List<Integer> res = new ArrayList<>();
		if (root == null)
			return res;
		Stack<TreeNode> stack = new Stack<TreeNode>();
		TreeNode cur = root;
		while (cur != null || !stack.isEmpty()) {
			while (cur != null) { // 一直往左子树走
				res.add(cur.val);
				stack.push(cur);
				cur = cur.left;
			}
			// 此时 cur == null，弹出栈顶元素，进入其右子节点
			cur = stack.pop().right;
		}
		return res;
	}
}
```