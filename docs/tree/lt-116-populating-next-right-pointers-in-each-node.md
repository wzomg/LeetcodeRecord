# 116.填充每个节点的下一个右侧节点指针
题目链接：[传送门](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)

## 题目描述：
给定一个完美二叉树，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

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

**提示**：

- 你只能使用常量级额外空间。
- 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

![](../_media/116_sample.png)

- 输入：

```
{"$id":"1","left":
{"$id":"2","left":
{"$id":"3","left":null,"next":null,"right":null,"val":4},"next":null,"right":
{"$id":"4","left":null,"next":null,"right":null,"val":5},"val":2},"next":null,"right":{"$id":"5","left":
{"$id":"6","left":null,"next":null,"right":null,"val":6},"next":null,"right":{"$id":"7","left":null,"next":null,"right":null,"val":7},"val":3},"val":1}
```

- 输出：

```
{"$id":"1","left":
{"$id":"2","left":{"$id":"3","left":null,"next":{"$id":"4","left":null,"next":{"$id":"5","left":null,"next":{"$id":"6","left":null,"next":null,"right":null,"val":7},"right":null,"val":6},"right":null,"val":5},"right":null,"val":4},"next":{"$id":"7","left":{"$ref":"5"},"next":null,"right":{"$ref":"6"},"val":3},"right":{"$ref":"4"},"val":2},"next":null,"right":{"$ref":"7"},"val":1}
```

- 解释：给定二叉树如图`A`所示，你的函数应该填充它的每个`next`指针，以指向其下一个右侧节点，如图`B`所示。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：在遍历当前层每个节点时顺便将下一层的节点链接成一个单链表，当遍历完时将cur指向下一层单链表的头节点继续遍历，直至遍历完整棵树即可。

## AC代码：
```java
class Solution {
	private Node head, nextCur;
	public Node connect(Node root) {
		Node cur = root;
		while (cur != null) {
			while (cur != null) {
				if (cur.left != null)
					build(cur.left);
				if (cur.right != null)
					build(cur.right);
				cur = cur.next;
			}
			cur = head;
			head = null;
		}
		return root;
	}
	private void build(Node node) {
		if (head == null) {
			nextCur = head = node;
		} else {
			nextCur.next = node;
			nextCur = nextCur.next;
		}
	}
}
```