# 725.分隔链表
题目链接：[传送门](https://leetcode-cn.com/problems/split-linked-list-in-parts/)

## 题目描述：
给你一个头结点为`head`的单链表和一个整数`k`，请你设计一个算法将链表分隔为`k`个连续的部分。

每部分的长度应该尽可能的相等：任意两部分的长度差距不能超过1。这可能会导致有些部分为null。

这k个部分应该按照在链表中出现的顺序排列，并且排在前面的部分的长度应该大于或等于排在后面的长度。

返回一个由上述 k 部分组成的数组。

**提示**：
- 链表中节点的数目在范围`[0, 1000]`
- $ 0 \leq$ `Node.val` $ \leq 1000$
- $1 \leq k \leq 50$

**示例**：

![](../_media/725-case.jpg)

- 输入：`head = [1,2,3]`, `k = 5`
- 输出：`[[1],[2],[3],[],[]]`
- 解释：第一个元素`output[0]`为`output[0].val = 1`，`output[0].next = null`。最后一个元素`output[4]`为`null`，但它作为 `ListNode`的字符串表示是`[]`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：简单链表。先统计链表长度，再分配元素即可。

## AC代码：
```go
func splitListToParts(head *ListNode, k int) []*ListNode {
	lenL := 0
	for cur := head; cur != nil; cur = cur.Next {
		lenL++
	}
	shang, yushu := lenL/k, lenL%k
	res := make([]*ListNode, 0)
	for cur := head; cur != nil; {
		itemHead := &ListNode{}
		itemCur := itemHead
		for cnt := shang; cnt > 0 && cur != nil; cnt-- {
			tmp := cur.Next
			itemCur.Next = cur
			itemCur = itemCur.Next
			cur = tmp
		}
		if yushu > 0 && cur != nil { // 均匀分配1个元素给前i个子数组
			tmp := cur.Next
			itemCur.Next = cur
			itemCur = itemCur.Next
			cur = tmp
			yushu--
		}
		itemCur.Next = nil //注意下一个节点置为空
		res = append(res, itemHead.Next)
	}
	for cnt := k - len(res); shang == 0 && cnt > 0; cnt-- {
		res = append(res, nil) // 不够长度，nil来凑
	}
	return res
}
```