# 101.对称二叉树
题目链接：[传送门](https://leetcode-cn.com/problems/symmetric-tree/)

## 题目描述：
给定一个二叉树，检查它是否是镜像对称的。

**进阶**：

你可以运用递归和迭代两种方法解决这个问题吗？

例如，二叉树`[1,2,2,3,4,4,3]`是对称的。

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个`[1,2,2,null,3,null,3]`则不是镜像对称的：

```
    1
   / \
  2   2
   \   \
   3    3
```

## 解决方案：
- 时间复杂度：$O(n)$，`n`为根节点的左右子树中节点数较少的个数
- 空间复杂度：$O(m)$，`m`为树的高度
- 思路：站在根节点往下看，分情况讨论结合树的递归套路即可写出！

## AC代码：
```java
class Solution {
	public boolean isSymmetric(TreeNode root) {
		if (root == null)
			return true;
		return check(root.left, root.right);
	}
	private boolean check(TreeNode t1, TreeNode t2) {
		// 站在根节点往下看左右子树，左右子节点分别为t1和t2
		// 先判断递归终止条件，只要一边有一边无就返回false，都为空则满足镜像
		if (t1 == null && t2 == null)
			return true;
		if (t1 == null || t2 == null)
			return false;
		// 当前t1和t2都不为空，先比较节点值再递归比较
		return t1.val == t2.val && check(t1.left, t2.right) && check(t1.right, t2.left);
	}
}
```