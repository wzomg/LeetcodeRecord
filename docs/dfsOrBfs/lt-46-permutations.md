# 46.全排列
题目链接：[传送门](https://leetcode-cn.com/problems/permutations/)

## 题目描述：
给定一个**没有重复**数字的序列，返回其所有可能的全排列。

**示例**:

- 输入: `[1,2,3]`
- 输出:

```
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

## 解决方案：
- 时间复杂度：$O(n × n!)$
- 空间复杂度：$O(n!)$
- 思路：暴搜！用一个标记数组来标记是否重复使用某个元素即可！

## AC代码：
```java
class Solution {
	private List<List<Integer>> res;
	private boolean[] vis;
	public List<List<Integer>> permute(int[] nums) {
		res = new ArrayList<List<Integer>>();
		int len;
		if (nums == null || (len = nums.length) == 0)
			return res;
		vis = new boolean[len];
		dfs(nums, len, new ArrayList<Integer>());
		return res;
	}
	private void dfs(int[] nums, int len, List<Integer> cur) {
		if (cur.size() == len) {
			res.add(new ArrayList<>(cur));
			return;
		}
		for (int i = 0; i < len; i++) {
			if (vis[i])
				continue;
			vis[i] = true;
			cur.add(nums[i]);
			dfs(nums, len, cur);
			cur.remove(cur.size() - 1);
			vis[i] = false;
		}
	}
}
```