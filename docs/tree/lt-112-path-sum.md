# 112.路径总和
题目链接：[传送门](https://leetcode-cn.com/problems/path-sum/)

## 题目描述：
给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

**说明**：叶子节点是指没有子节点的节点。

**示例**：给定如下二叉树，以及目标和`sum = 22`，返回`true`，因为存在目标和为`22`的根节点到叶子节点的路径`5->4->11->2`。

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：简单深搜。

## AC代码：
```java
class Solution {
	public boolean hasPathSum(TreeNode root, int sum) {
		if (root == null)
			return false;
		if (root.left == null && root.right == null && sum == root.val)
			return true;
		return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
	}
}
```