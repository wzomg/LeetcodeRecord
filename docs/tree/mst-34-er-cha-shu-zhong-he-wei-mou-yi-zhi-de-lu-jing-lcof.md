# 面试题34.二叉树中和为某一值的路径
题目链接：[传送门](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

## 题目描述：
输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

**提示**：节点总数 $ \leq 10000 $

**示例**:
给定如下二叉树，以及目标和`sum = 22`，

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```

**返回**:

```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：先序遍历二叉树。递归完左子树后要进入右子树就删除左子树中所有节点，其他作同样处理！

## AC代码：
```java
class Solution {
	private List<List<Integer>> res;
	public List<List<Integer>> pathSum(TreeNode root, int sum) {
		res = new ArrayList<List<Integer>>();
		dfs(root, sum, new ArrayList<Integer>());
		return res;
	}
	private void dfs(TreeNode root, int sum, List<Integer> cur) {
		if (root == null)
			return;
		cur.add(root.val);
		int pre = cur.size();
		sum -= root.val;
		if (sum == 0 && root.left == null && root.right == null) {
			// 若 sum 减为0，则查看当前节点是否为叶子节点，若是则添加答案，并且直接返回
			res.add(new ArrayList<Integer>(cur));
			return;
		}
		dfs(root.left, sum, cur);
		while (cur.size() > pre) // 删除左子树中的节点
			cur.remove(cur.size() - 1);
		dfs(root.right, sum, cur);
		while (cur.size() >= pre) // 删除右子树中的节点包括当前节点
			cur.remove(cur.size() - 1);
	}
}
```
