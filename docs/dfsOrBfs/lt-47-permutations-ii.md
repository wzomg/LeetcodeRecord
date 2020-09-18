# 47.全排列II
题目链接：[传送门](https://leetcode-cn.com/problems/permutations-ii/)

## 题目描述：
给定一个可包含**重复数字**的序列，返回所有**不重复的全排列**。

**示例**：

- 输入: `[1,1,2]`
- 输出:

```
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

## 解决方案：
- 时间复杂度：$O(n \times n!)$
- 空间复杂度：$O(n)$
- 思路：回溯算法。先排序，再深搜，相同相邻元素从第二个起没必要继续选出来再递归一次，直接剪枝掉。

## AC代码：
```java
class Solution {
	public List<List<Integer>> permuteUnique(int[] nums) {
		int len;
		List<List<Integer>> res = new ArrayList<List<Integer>>();
		if (nums == null || (len = nums.length) == 0)
			return res;
		// 先排序
		Arrays.sort(nums);
		boolean[] vis = new boolean[len];
		dfs(nums, len, res, new ArrayList<>(), vis);
		return res;
	}
	private void dfs(int[] nums, int len, List<List<Integer>> res, List<Integer> list, boolean[] vis) {
		if (list.size() == len) {
			res.add(new ArrayList<>(list));
			return;
		}
		for (int i = 0; i < len; i++) {
			// 若当前元素已被使用，则跳过
			// 或者 nums[i - 1] 在回退的过程中刚刚被撤销，即从第二个相同的元素起没必要继续选出来继续递归，不然会出现重复的情况
			if (vis[i] || (i > 0 && nums[i - 1] == nums[i] && !vis[i - 1]))
				continue;
			list.add(nums[i]);
			vis[i] = true;
			dfs(nums, len, res, list, vis);
			list.remove(list.size() - 1);
			vis[i] = false;
		}
	}
}
```