# 297.二叉树的序列化与反序列化
题目链接：[传送门](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

## 题目描述：
序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。

请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

**示例**： 
- 你可以将以下二叉树：

```
    1
   / \
  2   3
     / \
    4   5
```

- 序列化为 `"[1,2,3,null,null,4,5]"`

**说明**: 不要使用类的成员/全局/静态变量来存储状态，你的序列化和反序列化算法应该是无状态的。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(m)$
- 思路：简单操作二叉树，本题的关键点是将先序遍历每个节点的值与相邻节点用一个标志字符隔开，方便下次反序列化！

## AC代码：
```java
class Codec {
	public String serialize(TreeNode root) {
		if (root == null)
			return "#@";
		String str = String.valueOf(root.val) + "@";
		str += serialize(root.left);
		str += serialize(root.right);
		return str;
	}
	public TreeNode deserialize(String data) {
		String[] split = data.split("@");
        // 放进一个队列中，表示先序遍历二叉树的结果
		Queue<String> queue = new ArrayDeque<String>(); 
		for (int i = 0, len = split.length; i < len; i++)
			queue.add(split[i]);
        // 传递一个数组地址引用
		return dfs(queue);
	}
	private TreeNode dfs(Queue<String> queue) {
		if (queue.isEmpty())
			return null;
		String val = queue.remove();
		if (val.equals("#"))
			return null;
		TreeNode root = new TreeNode(Integer.valueOf(val));
		root.left = dfs(queue);
		root.right = dfs(queue);
		return root;
	}
}
```