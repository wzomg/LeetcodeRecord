# 199.二叉树的右视图
题目链接：[传送门](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

## 题目描述：
给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：先序遍历右子树，用哈希表记录每层是否已出现了第一个元素即可！

## AC代码：
```java
class Solution {
	private List<Integer> res;
	private Map<Integer, Boolean> vis;
	public List<Integer> rightSideView(TreeNode root) {
		res = new ArrayList<Integer>();
		vis = new HashMap<Integer, Boolean>();
		dfs(root, 0);
		return res;
	}
	private void dfs(TreeNode root, int level) {
		if (root == null)
			return;
		// 记录每层第一个被访问的节点
		if (!vis.containsKey(level)) {
			vis.put(level, true);
			res.add(root.val);
		}
        // 先序遍历右子树
		if (root.right != null)
			dfs(root.right, level + 1);
		dfs(root.left, level + 1);
	}
}
```
