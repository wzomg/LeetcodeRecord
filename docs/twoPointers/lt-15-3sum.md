# 15.三数之和
题目链接：[传送门](https://leetcode-cn.com/problems/3sum/)

## 题目描述：
给你一个包含`n`个整数的数组`nums`，判断`nums`中是否存在三个元素`a, b, c`，使得`a + b + c = 0`？请你找出所有满足条件且不重复的三元组。

**注意**：答案中不可以包含重复的三元组。

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n)$
- 思路：先排序，然后固定每个元素，双指针遍历寻找后面所有满足条件且不重复的2个元素，注意要去重！

## AC代码：
```java
class Solution {
	public List<List<Integer>> threeSum(int[] nums) {
		List<List<Integer>> res = new ArrayList<List<Integer>>();
		int len, lt, rt, sum;
		if (nums == null || (len = nums.length) == 0)
			return res;
		Arrays.sort(nums); // 先排序，再遍历
		for (int i = 0; i < len - 2; i++) {
            // 若当前元素大于0，则剩下的元素肯定不符合表达式
			if (nums[i] > 0)
				break;
			// 注意去重
			if (i - 1 >= 0 && nums[i] == nums[i - 1])
				continue;
			lt = i + 1;
			rt = len - 1;
			while (lt < rt) {
				sum = nums[i] + nums[lt] + nums[rt];
				if (sum < 0) // 只能增大b
					lt++;
				else if (sum > 0) // 只能减小c
					rt--;
				else {
					res.add(Arrays.asList(nums[i], nums[lt], nums[rt]));
					// 注意去重
					while (lt + 1 < rt && nums[lt] == nums[lt + 1])
						lt++;
					while (lt < rt - 1 && nums[rt - 1] == nums[rt])
						rt--;
					// 左右指针还要各自移动一个单位
					lt++;
					rt--;
				}
			}
		}
		return res;
	}
}
```