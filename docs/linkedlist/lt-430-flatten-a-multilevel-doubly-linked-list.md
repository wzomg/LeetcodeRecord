# 430.扁平化多级双向链表
题目链接：[传送门](https://leetcode-cn.com/problems/flatten-a-multilevel-doubly-linked-list/)

## 题目描述：
多级双向链表中，除了指向下一个节点和前一个节点指针之外，它还有一个子链表指针，可能指向单独的双向链表。这些子列表也可能会有一个或多个自己的子项，依此类推，生成多级数据结构，如下面的示例所示。

给你位于列表第一级的头节点，请你扁平化列表，使所有结点出现在单级双链表中。

**提示**：
- 节点数目不超过 1000
- $ 1 \leq$ `Node.val` $ \leq 10^5$

**示例**：
- 输入：`head = [1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]`
- 输出：`[1,2,3,7,8,11,12,9,10,4,5,6]`
- 解释：输入的多级列表如下图所示：

![](../_media/430-multilevellinkedlist.png)

扁平化后的链表如下图：

![](../_media/430-multilevellinkedlistflattened.png)


## 解决方案：
- 时间复杂度：$O(n)$，n为节点总数
- 空间复杂度：$O(m)$，m为孩子节点的深度
- 思路：递归深搜将每个节点的孩子节点紧接在其后，串接成一条双向链表，注意：递归返回的是最后一个节点。

## AC代码：
```go
func flatten(root *Node) *Node {
	dfs(root)
	return root
}
func dfs(cur *Node) *Node {
	var resLastNode *Node
	for cur != nil {
		three := cur.Next
    //有孩子节点则优先遍历
		if cur.Child != nil {
			childLastNode := dfs(cur.Child) //拿到孩子节点的最后一个节点
			if three != nil {               //将最后一个节点与cur 的下一个节点相连
				childLastNode.Next = three
				three.Prev = childLastNode
			}
			//将 child 的 pre 指向cur
			cur.Next = cur.Child
			cur.Child.Prev = cur
			cur.Child = nil // 记得重置当前节点的孩子节点为空
			resLastNode = childLastNode
		} else {
			resLastNode = cur
		}
		cur = three
	}
	return resLastNode
}
```