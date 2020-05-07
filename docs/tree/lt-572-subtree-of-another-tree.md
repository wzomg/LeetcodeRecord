# 572.另一个树的子树
题目链接：[传送门](https://leetcode-cn.com/problems/subtree-of-another-tree/)

## 题目描述：
给定两个非空二叉树`s`和`t`，检验`s`中是否包含和`t`具有相同结构和节点值的子树。`s`的一个子树包括`s`的一个节点和**这个节点的所有子孙**。`s`也可以看做它自身的一棵子树。

**示例1**:
- 给定的树`s`：

```
     3
    / \
   4   5
  / \
 1   2
```

- 给定的树`t`：

```
   4 
  / \
 1   2
```

- 返回`true`，因为`t`与`s`的一个子树拥有相同的结构和节点值。

**示例2**:
- 给定的树`s`：

```
     3
    / \
   4   5
  / \
 1   2
    /
   0
```

- 给定的树`t`：

```
   4 
  / \
 1   2
```

- 返回`false`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：先序遍历二叉树`s`，若当前节点`s.val == t.val`，则递归比较两棵树是否相同。注意：根节点相同的所有子孙节点必须完全相同，不能或多或少或不同。

## AC代码：
```java
class Solution {
	public boolean isSubtree(TreeNode s, TreeNode t) {
      // 若原本 t 为空，则 t 肯定是 s 的子树
		if (t == null) 
			return true;
      // 若 s 为空，此时 t 不为空，则 t 肯定不是 s 的子树
		if (s == null) 
			return false;
		return (s.val == t.val && isSame(s, t)) || isSubtree(s.left, t) || isSubtree(s.right, t);
	}
    // 单纯判断两棵树是否相同
	private boolean isSame(TreeNode s, TreeNode t) {
		if (s == null && t == null)
			return true;
		if (s == null || t == null)
			return false;
		return s.val == t.val && isSame(s.left, t.left) && isSame(s.right, t.right);
	}
}
```
