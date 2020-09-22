# 337.打家劫舍III
题目链接：[传送门](https://leetcode-cn.com/problems/house-robber-iii/)

## 题目描述：
在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。

**示例1**：

- 输入: `[3,2,3,null,3,null,1]`

```
     3
    / \
   2   3
    \   \ 
     3   1
```

- 输出: `7`
- 解释: 小偷一晚能够盗取的最高金额 `= 3 + 3 + 1 = 7`。

**示例2**:

- 输入: `[3,4,5,1,3,null,1]`

```
     3
    / \
   4   5
  / \   \ 
 1   3   1
```

- 输出: `9`
- 解释: 小偷一晚能够盗取的最高金额 `= 4 + 5 = 9`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：记忆化搜索。树形dp。比较简单，取和不取即可！

## AC代码：
```java
class Solution {
	private Map<TreeNode, Integer> map;
	public int rob(TreeNode root) {
		map = new HashMap<TreeNode, Integer>();
		return dfs(root);
	}
	private int dfs(TreeNode root) {
		if (root == null)
			return 0;
        // 记忆化，解决重复计算子问题
		if (map.containsKey(root))
			return map.get(root);
		int maxN = dfs(root.left) + dfs(root.right); // 不取当前节点，那么值为左右节点的值
		int maxY = 0;
		if (root.left != null) // 取当前节点，那么不能取子节点，只能取子孙节点的值
			maxY = dfs(root.left.left) + dfs(root.left.right);
		if (root.right != null)
			maxY += dfs(root.right.left) + dfs(root.right.right);
		int max = Math.max(maxN, maxY + root.val);
		map.put(root, max);
		return max; // 返回最大值
	}
}
```