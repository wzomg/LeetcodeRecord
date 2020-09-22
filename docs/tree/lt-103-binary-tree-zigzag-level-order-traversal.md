# 103.二叉树的锯齿形层次遍历
题目链接：[传送门](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

## 题目描述：
给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

**例如**：给定二叉树`[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回锯齿形层次遍历如下：

```
[
  [3],
  [20,9],
  [15,7]
]
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：双向队列的使用。层次遍历，从左往右，依次取出队首节点，左、右子节点依次添加到队尾；从右往左遍历，依次取出队尾节点，右、左子节点依次添加到队首。

## AC代码：
```java
class Solution {
	public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
		List<List<Integer>> res = new ArrayList<List<Integer>>();
		// 注意要先判空
		if (root == null)
			return res;
		Deque<TreeNode> queue = new LinkedList<TreeNode>();
		queue.add(root);
		// false：从左往右；true：从右往左
		boolean flag = false;
		TreeNode head;
		int siz;
		List<Integer> tmp;
		while (!queue.isEmpty()) {
			siz = queue.size();
			tmp = new ArrayList<Integer>();
			if (!flag) {
				while (siz-- > 0) {
					// 依次取出队首节点
					head = queue.removeFirst();
					tmp.add(head.val);
					if (head.left != null)
						queue.addLast(head.left);
					if (head.right != null)
						queue.addLast(head.right);
				}
			} else {
				while (siz-- > 0) {
					// 依次取出队尾节点
					head = queue.removeLast();
					// 从右往左遍历时，先将右子节点添加到队首，再添加左子节点到队首
					if (head.right != null)
						queue.addFirst(head.right);
					tmp.add(head.val);
					if (head.left != null)
						queue.addFirst(head.left);
				}
			}
			res.add(tmp);
			// 反转标志
			flag = !flag;
		}
		return res;
	}
}
```