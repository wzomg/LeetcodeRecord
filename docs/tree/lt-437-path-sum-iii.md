# 437.路径总和III
题目链接：[传送门](https://leetcode-cn.com/problems/path-sum-iii/)

## 题目描述：
给定一个二叉树的根节点`root`，和一个整数`targetSum`，求该二叉树里节点值之和等于`targetSum`的**路径**的数目。

**路径**不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

**提示**：
- 二叉树的节点个数的范围是`[0,1000]`
- $-10^9 \leq$ `Node.val` $ \leq 10^9$ 
- $-1000 \leq$ `targetSum` $ \leq 1000$ 

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：树的前缀和+计数dp。注意：返回到上一个节点前要将当前前缀和个数减去1。

## AC代码：
```go
type TreeNode struct {
	Val   int
	Left  *TreeNode
	Right *TreeNode
}
func pathSum(root *TreeNode, targetSum int) int {
	return dfs(root, targetSum, 0, map[int]int{0: 1})
}
func dfs(root *TreeNode, targetSum int, curSum int, cnt map[int]int) int { // curSum表示根节点到当前节点的前缀和
	if root == nil { // 终止条件
		return 0
	}
	res := 0
	curSum += root.Val                            // 统计前缀和
	res += cnt[curSum-targetSum]                  // res为答案
	cnt[curSum]++                                 // 更新当前前缀和个数加1
	res += dfs(root.Left, targetSum, curSum, cnt) // 继续递归左右子树
	res += dfs(root.Right, targetSum, curSum, cnt)
	cnt[curSum]-- // 回到当前节点，需要将当前节点的前缀和出现的个数减去1
	return res    // 返回当前统计的结果
}
```