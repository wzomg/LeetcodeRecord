# 350.两个数组的交集II
题目链接：[传送门](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)

## 题目描述：
给定两个数组，编写一个函数来计算它们的交集。

**说明**：
- 输出结果中每个元素出现的次数，应与元素在两个数组中出现次数的最小值一致。
- 我们可以不考虑输出结果的顺序。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：用2个哈希表统计各自数组中每个元素出现的个数，然后再遍历其中一个哈希表取出交集的元素即可！

## AC代码：
```java
class Solution {
	public int[] intersect(int[] nums1, int[] nums2) {
		int len1, len2;
		if (nums1 == null || nums2 == null || (len1 = nums1.length) == 0 || (len2 = nums2.length) == 0)
			return new int[0];
		Map<Integer, Integer> map1 = new HashMap<>();
		Map<Integer, Integer> map2 = new HashMap<>();
		for (int i = 0; i < len1; i++)
			map1.put(nums1[i], map1.getOrDefault(nums1[i], 0) + 1);
		for (int i = 0; i < len2; i++)
			map2.put(nums2[i], map2.getOrDefault(nums2[i], 0) + 1);
		List<Integer> res = new ArrayList<>();
		for (Integer k : map1.keySet()) {
			if (map2.containsKey(k)) {
				int mina = Math.min(map1.get(k), map2.get(k));
				for (int i = 0; i < mina; i++)
					res.add(k);
			}
		}
		int[] ans = new int[res.size()];
		for (int i = 0, len = res.size(); i < len; i++) {
			ans[i] = res.get(i);
		}
		return ans;
	}
}
```