# 128.最长连续序列
题目链接：[传送门](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

## 题目描述：
给定一个未排序的整数数组，找出最长连续序列的长度。

要求算法的时间复杂度为 $O(n)$。

## 解决方案：
- 时间复杂度：$O(n) + O(\alpha(n))$
- 空间复杂度：$O(n)$
- 思路：并查集。先遍历去重元素，再寻找每个元素的是否存在`nums[i]-1`或者`nums[i]+1`，有就合并成在一个连通块，最后取连通块中最多的节点数即可！

## AC代码：
```java
class Solution {
	private Map<Integer, Integer> father;
	private Map<Integer, Integer> cnt;
	public int longestConsecutive(int[] nums) {
		int len, res = 0, idx = 0; // idx 表示不用元素的个数
		if (nums == null || (len = nums.length) == 0)
			return 0;
		father = new HashMap<Integer, Integer>();
		cnt = new HashMap<Integer, Integer>();
		for (int i = 0; i < len; i++) {
			if (!father.containsKey(nums[i]))
				nums[idx++] = nums[i];
			father.put(nums[i], nums[i]); // 每个节点的值初始化为本身
		}
		for (int i = 0; i < idx; i++) { // 分别找nums[i]-1和nums[i]+1，有就合并
			if (father.containsKey(nums[i] - 1))
				unite(nums[i], nums[i] - 1);
			if (father.containsKey(nums[i] + 1))
				unite(nums[i], nums[i] + 1);
		}
		for (int i = 0; i < idx; i++) { // 注意去重，否则答案会比实际值大
			int fa = find_father(nums[i]);
			cnt.put(fa, cnt.getOrDefault(fa, 0) + 1);
			res = Math.max(res, cnt.get(fa));
		}
		return res;
	}
	private int find_father(int x) {
		int fa = father.get(x);
        // father[x] = find_father(father[x]); // 路径压缩
		if (fa != x)
			fa = find_father(fa);
		father.put(x, fa); 
		return fa;
	}
	private void unite(int x, int y) {
		x = find_father(x);
		y = find_father(y);
		if (x != y)
			father.put(x, y);
	}
}
```