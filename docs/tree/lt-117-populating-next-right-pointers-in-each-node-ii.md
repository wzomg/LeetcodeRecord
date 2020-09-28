# 117.填充每个节点的下一个右侧节点指针II
题目链接：[传送门](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/)

## 题目描述：
给定一个二叉树：

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个`next`指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将`next`指针设置为`NULL`。

初始状态下，所有`next`指针都被设置为`NULL`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$ 或 $O(1)$
- 思路：层次遍历。2种做法：①用一个队列来存储待遍历的层节点。②在遍历当前层每个节点时顺便将下一层的节点链接成一个单链表，当遍历完时将cur指向下一层单链表的头节点继续遍历，直至遍历完整棵树即可。

## AC代码1：
```java
class Solution {
	public Node connect(Node root) {
		if (root == null)
			return null;
		Queue<Node> queue = new LinkedList<Node>();
		queue.add(root);
		Node pre, cur;
		int siz;
		while (!queue.isEmpty()) {
			siz = queue.size();
			pre = null;
			for (int i = 0; i < siz; i++) {
				cur = queue.remove();
				if (cur.left != null)
					queue.add(cur.left);
				if (cur.right != null)
					queue.add(cur.right);
				if (i > 0)
					pre.next = cur;
				pre = cur;
			}
		}
		return root;
	}
}
```

## AC代码2：
```java
class Solution {
	public Node connect(Node root) {
		if (root == null)
			return null;
		Node cur = root;
		Node dummy, tail;
		while (cur != null) {
			tail = dummy = new Node();
			// 遍历当前层
			while (cur != null) {
				if (cur.left != null) {
					tail.next = cur.left;
					tail = tail.next;
				}
				if (cur.right != null) {
					tail.next = cur.right;
					tail = tail.next;
				}
				cur = cur.next;
			}
			cur = dummy.next;
		}
		return root;
	}
}
```