# 78.子集
题目链接：[传送门](https://leetcode-cn.com/problems/subsets/)

## 题目描述：
给定一组**不含重复元素**的整数数组`nums`，返回该数组所有可能的子集（幂集）。

**说明**：解集不能包含重复的子集。

**示例**:

- 输入: `nums = [1,2,3]`
- 输出:

```
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

## 解决方案：
- 时间复杂度：$O(2^n)$，n为数组元素个数
- 空间复杂度：$O(2^n)$
- 思路：取 $0 \sim 2^n-1 $ 每个数对应二进制数中'1'对应的值即可。

## AC代码：
```java
class Solution {
	public List<List<Integer>> subsets(int[] nums) {
		int len;
		List<List<Integer>> res = new ArrayList<List<Integer>>();
		if (nums == null || (len = nums.length) == 0)
			return res;
		List<Integer> tmp;
		for (int i = 0, siz = 1 << len; i < siz; i++) {
			tmp = new ArrayList<>();
			for (int j = 0; j < len; j++) {
				if (((i >> j) & 1) == 1)
					tmp.add(nums[j]);
			}
			res.add(tmp);
		}
		return res;
	}
}
```