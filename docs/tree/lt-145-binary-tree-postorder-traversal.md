# 145.二叉树的后序遍历
题目链接：[传送门](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

## 题目描述：
给定一个二叉树，返回它的**后序**遍历。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：递归法：左→右→根。非递归法：先序遍历的变形，原先是：根→左→右，那么把先序遍历改成：根→右→左，再逆序一下就是后序遍历的结果。

## AC代码1：递归法。
```java
class Solution {
	public List<Integer> postorderTraversal(TreeNode root) {
		List<Integer> res = new ArrayList<>();
		if (root == null)
			return res;
		dfs(root, res);
		return res;
	}
	private void dfs(TreeNode root, List<Integer> cur) {
		if (root == null)
			return;
		dfs(root.left, cur);
		dfs(root.right, cur);
		cur.add(root.val);
	}
}
```

## AC代码2：非递归法。
```java
class Solution {
	public List<Integer> postorderTraversal(TreeNode root) {
		List<Integer> res = new LinkedList<>(); // 使用双向链表，插入时间复杂度为O(1)
		if (root == null)
			return res;
		Stack<TreeNode> stack = new Stack<TreeNode>();
		TreeNode cur = root;
		while (cur != null || !stack.isEmpty()) {
			while (cur != null) {
				stack.add(cur); // 一直往右子节点走
				res.add(0, cur.val); // 每次在下标0位置插入该元素
				cur = cur.right;
			}
			// cur == null
			cur = stack.pop().left;
		}
		return res;
	}
}
```