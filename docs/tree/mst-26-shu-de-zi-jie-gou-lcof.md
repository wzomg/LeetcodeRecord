# 面试题26.树的子结构
题目链接：[传送门](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

## 题目描述：
输入两棵二叉树`A`和`B`，判断`B`是不是`A`的子结构。(**约定空树不是任意一个树的子结构**)

`B`是`A`的子结构， 即`A`中有出现和`B`相同的结构和节点值。

**限制**：$ 0 \leq $ 节点个数 $ \leq 10000 $

**例如**：

- 给定的树`A`:

```
     3
    / \
   4   5
  / \
 1   2
```

- 给定的树`B`：

```
   4 
  /
 1
```

- 返回`true`，因为`B`与`A`的一个子树拥有相同的结构和节点值。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$，`m`为树的高度
- 思路：先序遍历二叉树`A`，若当前节点`A.val == B.val`，则递归比较`B`是否为`A`的子结构。注意是包含关系，即树`B`的结构和值有部分与`A`完全相同即可。

## AC代码：
```java
class Solution {
	public boolean isSubStructure(TreeNode A, TreeNode B) {
		// 约定空树不是任意一个树的子结构，只要B为空，返回false。若A为空，B不为空，也返回false
		if (A == null || B == null)
			return false;
		// 若根节点值相同才进入另外一个函数遍历比较，否则遍历树A的每个节点的值，有相同就递归比较
		return (A.val == B.val && isHas(A, B)) || isSubStructure(A.left, B) || isSubStructure(A.right, B);
	}
	// 判断 r2 是否为 r1 的子结构
	private boolean isHas(TreeNode r1, TreeNode r2) {
		// 只要r2遍历完，无论 r1 是否遍历完，r2 都是 r1 的子结构
		if (r2 == null)
			return true;
		// 若 r1 遍历完，此时 r2 肯定不为空，则 r2 肯定不是 r1 的子结构！
		if (r1 == null)
			return false;
		// 否则继续递归比较
		return r1.val == r2.val && isHas(r1.left, r2.left) && isHas(r1.right, r2.right);
	}
}
```