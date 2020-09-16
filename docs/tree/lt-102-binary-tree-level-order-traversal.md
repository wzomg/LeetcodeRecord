# 102.二叉树的层序遍历
题目链接：[传送门](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

## 题目描述：
给你一个二叉树，请你返回其按**层序遍历**得到的节点值。（即逐层地，从左到右访问所有节点）。

**示例**：

- 二叉树：`[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

- 返回其层次遍历结果：

```
[
  [3],
  [9,20],
  [15,7]
]
```

## 解决方案：
- 时间复杂度：$O(n)$，`n`为二叉树的总节点个数
- 空间复杂度：$O(n)$，`n`为二叉树的总节点个数
- 思路：广度优先搜索，按层遍历，每次先计算队列的个数`cnt`，再循环添加其子节点即可！注意：一开始需要判断根节点是否为空！

## AC代码：
```java
class Solution {
	public List<List<Integer>> levelOrder(TreeNode root) {
		List<List<Integer>> res = new ArrayList<List<Integer>>();
		if (root == null)
			return res;
		Queue<TreeNode> queue = new LinkedList<TreeNode>();
		queue.add(root);
		TreeNode curNode;
		int cnt;
		List<Integer> list;
		while (!queue.isEmpty()) {
			cnt = queue.size();
			list = new ArrayList<Integer>();
			while (cnt-- > 0) { // 处理完当前一层所有节点
				curNode = queue.poll();
				list.add(curNode.val);
				if (curNode.left != null)
					queue.add(curNode.left);
				if (curNode.right != null)
					queue.add(curNode.right);
			}
			res.add(list);
		}
		return res;
	}
}
```