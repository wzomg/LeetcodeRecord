# 501.二叉搜索树中的众数
题目链接：[传送门](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)

## 题目描述：
给定一个有相同值的二叉搜索树（`BST`），找出`BST`中的所有众数（出现频率最高的元素）。

假定`BST`有如下定义：

- 结点左子树中所含结点的值小于等于当前结点的值
- 结点右子树中所含结点的值大于等于当前结点的值
- 左子树和右子树都是二叉搜索树

**提示**：如果众数超过1个，不需考虑输出顺序。

**进阶**：你可以不使用额外的空间吗？（假设由递归产生的隐式调用栈的开销不被计算在内）。

**示例**：给定 BST：`[1,null,2,2]`，返回`[2]`。

```
   1
    \
     2
    /
   2
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：`Morris`中序遍历，边遍历边更新众数列表即可。它的一个重要思想就是寻找当前节点的前驱节点，并且`Morris`中序遍历寻找下一个点始终是通过转移到`right`指针指向的位置来完成的。具体的算法步骤如下：

```
1、根据当前节点，找到其前驱节点，若前驱节点的右子节点为空，则把前驱节点的右子节点指向当前节点，然后进入当前节点的左子节点；
2、若当前节点的左子节点为空，则打印当前节点，然后进入右子节点；
3、若当前节点的前驱节点的右子节点指向了它本身，则把前驱节点的右子节点设置为空（还原树原本结构），打印当前节点，然后进入右子节点。
4、按照以上规则，直到遍历完所有节点为止！
```

## AC代码：
```java
class Solution {
	private List<Integer> list;
	private int data, cnt, maxNum;
	boolean flag;
	public int[] findMode(TreeNode root) {
		list = new ArrayList<Integer>();
		// 当前节点cur，pre 为 cur 的前驱节点
		TreeNode cur = root, pre;
		maxNum = 0;
		flag = false;
		while (cur != null) {
			// 若当前节点没有左子树，则遍历这个点，然后跳转到当前节点的右子树。
			if (cur.left == null) {
				update(cur.val);
				cur = cur.right;
			} else {
				// 否则将pre定位为左子节点，去寻找cur的前驱节点，也就是左子树右下方最后一个叶子节点
				pre = cur.left;
				while (pre.right != null && pre.right != cur)
					pre = pre.right;
				// 是否建立线索还是划掉线索
				if (pre.right == null) {
					// 建立线索
					pre.right = cur;
					// 进入左子节点继续建立线索
					cur = cur.left;
				} else if (pre.right == cur) {
					// 划掉线索，说明已经遍历前驱节点指向根，下一步进入右子节点继续遍历
					pre.right = null;
					update(cur.val);
					// 进入右子节点
					cur = cur.right;
				}
			}
		}
		int[] res = new int[list.size()];
		for (int i = 0, siz = list.size(); i < siz; i++)
			res[i] = list.get(i);
		return res;
	}
	private void update(int val) {
		if (!flag) {
			data = val;
			cnt = 1;
			flag = true;
		} else {
			if (data == val) {
				cnt++;
			} else {
				data = val;
				cnt = 1;
			}
		}
		// 若当前元素出现次数相同，则直接添加到列表中
		if (cnt == maxNum) {
			list.add(val);
		} else if (cnt > maxNum) {
			maxNum = cnt;
			list.clear();
			list.add(val);
		}
	}
}
```