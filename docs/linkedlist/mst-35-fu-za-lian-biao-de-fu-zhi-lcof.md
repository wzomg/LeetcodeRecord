# 面试题35.复杂链表的复制
题目链接：[传送门](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

## 题目描述：
请实现`copyRandomList`函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个`next`指针指向下一个节点，还有一个`random`指针指向链表中的任意节点或者`null`。

**提示**：

- $-10000 \leq $ `Node.val` $ \leq 10000$
- `Node.random` 为空（`null`）或指向链表中的节点。
- 节点数目不超过`1000`。

**示例1**：

![](../_media/e1.png)

- 输入：`head = [[7,null],[13,0],[11,4],[10,2],[1,0]]`
- 输出：`[[7,null],[13,0],[11,4],[10,2],[1,0]]`

**示例2**：

![](../_media/e2.png)

- 输入：`head = [[1,1],[2,1]]`
- 输出：`[[1,1],[2,1]]`

**示例3**：

![](../_media/e3.png)

- 输入：`head = [[3,null],[3,0],[3,null]]`
- 输出：`[[3,null],[3,0],[3,null]]`

**示例4**：

- 输入：`head = []`
- 输出：`[]`
- 解释：给定的链表为空（空指针），因此返回`null`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：分三步走：①复制出每个节点并粘在其后面；②复制新节点的任意节点指向；③将奇数、偶数位置的节点各自连成一条单链！

## AC代码：
```java
class Solution {
	public Node copyRandomList(Node head) {
		if (head == null)
			return null;
		// 1、每个节点都复制一个并粘在其后面
		Node cur = head, newNode;
		while (cur != null) {
			newNode = new Node(cur.val);
			newNode.next = cur.next;
			cur.next = newNode;
			cur = newNode.next;
		}
		// 2、将新new出来的每个节点的一个任意指针指向正确位置
		cur = head;
		while (cur != null) {
			if (cur.random != null)
				cur.next.random = cur.random.next;
			cur = cur.next.next;
		}
		// 3、复制出偶数节点，并且还原奇数上节点的链表，否则会出错
		Node newHead = head.next, tmp;
		cur = head;
		while (cur != null) {
			// 先保存偶数节点
			tmp = cur.next;
			// 连接好下一个奇数节点
			cur.next = tmp.next;
			// 连接好下一个偶数节点
			tmp.next = cur.next == null ? null : cur.next.next;
			// 移动到下一个奇数节点
			cur = cur.next;
		}
		return newHead;
	}
}
```