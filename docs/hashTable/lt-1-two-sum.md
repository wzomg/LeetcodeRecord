# 1.两数之和
题目链接：[传送门](https://leetcode-cn.com/problems/two-sum/)

## 题目描述：
给定一个整数数组`nums`和一个目标值`target`，请你在该数组中找出和为目标值的那**两个**整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

**示例**:

给定 `nums = [2, 7, 11, 15]`, `target = 9`，因为 `nums[0] + nums[1] = 2 + 7 = 9`，所以返回 `[0, 1]`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：采用边哈希边查找的方式来判断是否有相异下标的2个元素和为`target`即可！

## AC代码：
```java
class Solution {
	public int[] twoSum(int[] nums, int target) {
		int len;
		if (nums == null || (len = nums.length) == 0)
			return new int[] { -1, -1 };
		Map<Integer, Integer> map = new HashMap<>();
		for (int i = 0; i < len; i++) {
			if (map.containsKey(target - nums[i]))
				return new int[] { map.get(target - nums[i]), i };
			map.put(nums[i], i);
		}
		return new int[] { -1, -1 };
	}
}
```

```go
func twoSum(nums []int, target int) []int {
	var res = []int{-1, -1}
	var link = make(map[int]int)
	for idx, val := range nums {
		cha := target - val
		if preIdx, ok := link[cha]; ok {
			res[0] = preIdx
			res[1] = idx
		} else {
			link[val] = idx
		}
	}
	return res
}
```