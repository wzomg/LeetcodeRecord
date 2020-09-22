# 968.监控二叉树
题目链接：[传送门](https://leetcode-cn.com/problems/binary-tree-cameras/)

## 题目描述：
给定一个二叉树，我们在树的节点上安装摄像头。

节点上的每个摄影头都可以监视其**父对象、自身及其直接子对象**。

计算监控树的所有节点所需的最小摄像头数量。

**提示**：

- 给定树的节点数的范围是`[1, 1000]`。
- 每个节点的值都是`0`。

**示例1**：

![](../_media/bst_cameras_01.png)

- 输入：`[0,0,null,0,0]`
- 输出：`1`
- 解释：如图所示，一台摄像头足以监控所有节点。

**示例2**：

![](../_media/bst_cameras_02.png)

- 输入：`[0,0,null,0,null,0,null,null,0]`
- 输出：`2`
- 解释：需要至少两个摄像头来监视树的所有节点。上图显示了摄像头放置的有效位置之一。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：贪心。定义3种状态：1、安装了摄像头；2、已被监视；3、未被监视。为了使得安装摄像头的数量最少，采用回溯的方式自底向上依次查看某些节点是否需要设置摄像头即可，详解看代码。

## AC代码：
```java
class Solution {
	private int num;
	public int minCameraCover(TreeNode root) {
		num = 0;
		if (dfs(root) == 3)
			num++;
		return num;
	}
	// 3种状态：1、安装了摄像头；2、已被监视；3、未被监视；
	private int dfs(TreeNode root) {
		// 节点为空，设置为已被监视
		if (root == null) {
			return 2;
		}
		// 递归获取左右子节点的状态
		int lt = dfs(root.left);
		int rt = dfs(root.right);
		// 1、要先判断子节点是否被监视，若其中有一个未被监视，则在当前节点安装一个摄像头
		if (lt == 3 || rt == 3) {
			num++;
			return 1;
		}
		// 2、再判断子节点是否安装了摄像头，是则当前节点已被监视
		if (lt == 1 || rt == 1) {
			return 2;
		}
		// 3、当前节点未被监视，最有可能是叶子节点，或者左右子节点都被监视
		return 3;
	}
}
```