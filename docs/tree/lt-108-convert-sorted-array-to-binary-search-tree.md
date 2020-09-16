# 108.将有序数组转换为二叉搜索树
题目链接：[传送门](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

## 题目描述：
将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过`1`。

**示例**：

- 给定有序数组: `[-10,-3,0,5,9]`，一个可能的答案是：`[0,-3,9,-10,null,5]`，它可以表示下面这个高度平衡二叉搜索树：

```
      0
     / \
   -3   9
   /   /
 -10  5
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(log^n)$
- 思路：按中序遍历的方式构造一棵二叉搜索树即可。

## AC代码：
```java
class Solution {
	public TreeNode sortedArrayToBST(int[] nums) {
		int len;
		if (nums == null || (len = nums.length) == 0)
			return null;
		return dfs(nums, 0, len - 1);
	}
	private TreeNode dfs(int[] nums, int left, int right) {
		if (left > right)
			return null;
		int mid = (left + right) >> 1;
		TreeNode root = new TreeNode(nums[mid]);
		root.left = dfs(nums, left, mid - 1);
		root.right = dfs(nums, mid + 1, right);
		return root;
	}
}
```