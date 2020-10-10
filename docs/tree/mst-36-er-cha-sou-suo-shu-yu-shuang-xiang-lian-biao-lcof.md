# 面试题36.二叉搜索树与双向链表
题目链接：[传送门](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

## 题目描述：
输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。为了让您更好地理解问题，以下面的二叉搜索树为例：

![](../_media/bstdlloriginalbst.png)

我们希望将这个二叉搜索树转化为双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。

下图展示了上面的二叉搜索树转化成的链表。“head” 表示指向链表中有最小元素的节点。

![](../_media/bstdllreturndll.png)

特别地，我们希望可以就地完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中的第一个节点的指针。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：考察树的中序遍历！每次将当前节点的左指针指向最左下最后一个节点，然后`last`的右指针指向当前节点。

## AC代码：
```java
class Solution {
	public Node treeToDoublyList(Node root) {
		if (root == null)
			return null;
		Node lastNode = dfs(root, null);
		Node cur = lastNode;
		while (cur != null && cur.left != null) // 一直往左指针找到第一个节点
			cur = cur.left;
		cur.left = lastNode; // 第一个节点的左指针为空
		lastNode.right = cur; // 最后一个节点的右指针为空
		return cur;
	}
	private Node dfs(Node root, Node lastNode) {
		if (root == null)
			return null;
		// 先序遍历到最左下的一个叶子节点
		if (root.left != null)
			lastNode = dfs(root.left, lastNode);
		// 将当前节点左指针指向值小于root.val但是左子树中最大的节点
		root.left = lastNode;
		if (lastNode != null) // 注意判空
			lastNode.right = root; // 将该节点的右指针指向root
		lastNode = root; // 最后一个指针时刻定位到当前节点，然后递归遍历右子树
		// 同样是递归到叶子节点即可！
		if (root.right != null)
			lastNode = dfs(root.right, lastNode);
		return lastNode; // 返回当前最左下的一个节点
	}
}
```