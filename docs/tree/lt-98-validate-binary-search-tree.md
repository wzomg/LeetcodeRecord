# 98.验证二叉搜索树
题目链接：[传送门](https://leetcode-cn.com/problems/validate-binary-search-tree/)

## 题目描述：
给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

节点的左子树只包含**小于**当前节点的数。

节点的右子树只包含**大于**当前节点的数。

所有左子树和右子树自身必须也是二叉搜索树。

**示例**:

- 输入:

```
    5
   / \
  1   4
     / \
    3   6
```

- 输出: `false`
- 解释: 输入为: `[5,1,4,null,null,3,6]`。根节点的值为`5`，但是其右子节点值为`4`。

## 解决方案：
- 时间复杂度：$O(n)$，n为树中节点总数
- 空间复杂度：$O(m)$，m为树的高度
- 思路：验证是否为一棵二叉搜索树，常采用**中序遍历**的递归方式，每次记录上一个遍历到的节点值`pre`，当满足`pre < cur.val`，且验证右子树的返回值为`true`才满足最终得到一个**单调递增序列**！

## AC代码：
```java
class Solution {
	private long pre = -2147483649L; // 坑点，边界值
	public boolean isValidBST(TreeNode root) {
		// 空树也满足二叉搜索树，终止条件：空节点
		if (root == null)
			return true;
		if (isValidBST(root.left)) { // 验证左子树必须为true
            // 不能出现有相同值的节点，且必须满足左值<根值，然后再递归验证右子树是否正确
			if (pre < root.val) { 
				pre = root.val;
				return isValidBST(root.right);
			}
		}
		return false;
	}
}
```
