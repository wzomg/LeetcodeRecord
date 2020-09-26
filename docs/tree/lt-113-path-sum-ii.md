# 113.路径总和II
题目链接：[传送门](https://leetcode-cn.com/problems/path-sum-ii/)

## 题目描述：
给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。

**说明**：叶子节点是指没有子节点的节点。

**示例**：给定如下二叉树，以及目标和`sum = 22`，

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```

返回：

```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：先序遍历。详解看代码注释。

## AC代码：
```java
class Solution {
	public List<List<Integer>> pathSum(TreeNode root, int sum) {
		List<List<Integer>> res = new ArrayList<List<Integer>>();
		dfs(root, sum, res, new ArrayList<>());
		return res;
	}
	private void dfs(TreeNode root, int sum, List<List<Integer>> res, List<Integer> list) {
		if (root == null)
			return;
		list.add(root.val);
		if (root.left == null && root.right == null && sum == root.val) {
			res.add(new ArrayList<>(list));
			return;
		}
		int siz = list.size();
		dfs(root.left, sum - root.val, res, list);
		// 删除左子树中的所有节点
		while (list.size() > siz) {
			list.remove(list.size() - 1);
		}
		dfs(root.right, sum - root.val, res, list);
		// 删除右子树中的所有节点包括当前节点，返回到当前节点的父节点
		while (list.size() >= siz) {
			list.remove(list.size() - 1);
		}
	}
}
```