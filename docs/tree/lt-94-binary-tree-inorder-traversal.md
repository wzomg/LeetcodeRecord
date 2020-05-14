# 94.二叉树的中序遍历
题目链接：[传送门](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

## 题目描述：
给定一个二叉树，返回它的**中序**遍历。

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

- 输出: `[1,3,2]`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$，$O(n)$
- 思路：递归法：左→根→右。非递归法：借助栈数据结构或者使用二叉树的Morris遍历算法。后者：若单纯打印节点值，则其空间复杂度为：$O(1)$。其算法步骤如下：

```
1、根据当前节点，找到其前驱节点，若前驱节点的右子节点为空，则把前驱节点的右子节点指向当前节点，然后进入当前节点的左子节点；
2、若当前节点的左子节点为空，则打印当前节点，然后进入右子节点；
3、若当前节点的前驱节点的右子节点指向了它本身，则把前驱节点的右子节点设置为空（还原树原本结构），打印当前节点，然后进入右子节点。
4、按照以上规则，直到遍历完所有节点为止！
```

## AC代码1：递归法。
```java
class Solution {
	public List<Integer> inorderTraversal(TreeNode root) {
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
		cur.add(root.val);
		dfs(root.right, cur);
	}
}
```

## AC代码2：非递归法，借助栈。
```java
class Solution {
	public List<Integer> inorderTraversal(TreeNode root) {
		List<Integer> res = new ArrayList<>();
		if (root == null)
			return res;
		Stack<TreeNode> stack = new Stack<TreeNode>();
		TreeNode cur = root;
		while (cur != null || !stack.isEmpty()) {
			while (cur != null) { // 一直往左子节点走
				stack.push(cur); // 并将当前节点入栈 
				cur = cur.left; 
			}
			// cur == null
			cur = stack.pop(); // 弹出栈顶元素并打印
			res.add(cur.val);
			cur = cur.right; // 左→根→右
		}
		return res;
	}
}
```

## AC代码3：非递归法，二叉树的Morris遍历。
```java
class Solution {
	public List<Integer> inorderTraversal(TreeNode root) {
		List<Integer> res = new ArrayList<Integer>();
		TreeNode cur = root, pre; // cur：当前节点，pre：cur的前驱节点
		while (cur != null) {
			if (cur.left == null) {
				// 若当前节点的左子节点为空，则打印当前节点，然后进入右子节点（按中序遍历方式，左根右）
				res.add(cur.val);
				cur = cur.right;
			} else {
				pre = cur.left; // 否则将pre定位为左子节点，去寻找cur的前驱节点
				while (pre.right != null && pre.right != cur)
					pre = pre.right;
				// 是否建立线索还是划掉线索
				if (pre.right == null) {
					pre.right = cur; // 建立线索
					cur = cur.left; // 并进入左子节点
				} else if (pre.right == cur) {
					pre.right = null; // 划掉线索，说明已经遍历到根，下一步进入右子节点继续遍历
					res.add(cur.val); // 打印当前节点的值
					cur = cur.right; // 进入右子节点
				}
			}
		}
		return res;
	}
}
```

