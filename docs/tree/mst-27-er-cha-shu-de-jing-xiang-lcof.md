# 面试题27.二叉树的镜像
题目链接：[传送门](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

## 题目描述：
请完成一个函数，输入一个二叉树，该函数输出它的镜像。

**限制**：$0 \leq$ 节点个数 $ \leq 1000$

- 例如输入：

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

- 镜像输出：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

**示例**：

- 输入：`root = [4,2,7,1,3,6,9]`
- 输出：`[4,7,2,9,6,3,1]`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$，m为树的高度
- 思路：先序遍历。站在父节点上，因为二叉树的递归具有相似处理，故交换一下左右儿子节点即可！

## AC代码：
```java
class Solution {
	public TreeNode mirrorTree(TreeNode root) {
		if (root == null)
			return null;
		TreeNode lt = mirrorTree(root.left);
		TreeNode rt = mirrorTree(root.right);
		root.left = rt;
		root.right = lt;
		return root;
	}
}
```