# 18.四数之和
题目链接：[传送门](https://leetcode-cn.com/problems/4sum/)

## 题目描述：
给定一个包含`n`个整数的数组`nums`和一个目标值`target`，判断`nums`中是否存在四个元素`a`，`b`，`c`和`d`，使得 $a + b + c + d$ 的值与`target`相等？找出所有满足条件且不重复的四元组。

**注意**：答案中不可以包含重复的四元组。

**示例**：

- 给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。
- 满足要求的四元组集合为：
```
[
    [-1,  0, 0, 1],
    [-2, -1, 1, 2],
    [-2,  0, 0, 2]
]
```

## 解决方案：
- 时间复杂度：$O(n^3)$
- 空间复杂度：$O(n)$
- 思路：类似于三数之和，先排序，每次固定`a`，`b`指针，然后在区间`[b+1, len-1]`之间用双指针求`nums[a]+nums[b]+nums[c]+nums[d]==target`即可！注意：去重！

## AC代码：
```java
class Solution {
	public List<List<Integer>> fourSum(int[] nums, int target) {
		int len, sum;
		List<List<Integer>> res = new ArrayList<List<Integer>>();
		if (nums == null || (len = nums.length) < 4)
			return res;
		Arrays.sort(nums);
		for (int a = 0, c, d; a < len - 3; a++) {
			if (a > 0 && nums[a - 1] == nums[a])
				continue;
			for (int b = a + 1; b < len - 2; b++) {
				if (b > a + 1 && nums[b - 1] == nums[b])
					continue;
				c = b + 1;
				d = len - 1;
				while (c < d) {
					sum = nums[a] + nums[b] + nums[c] + nums[d];
					if (sum < target)
						c++;
					else if (sum > target)
						d--;
					else { // ==
						res.add(Arrays.asList(nums[a], nums[b], nums[c], nums[d]));
						while (c + 1 < d && nums[c] == nums[c + 1])
							c++;
						while (c < d - 1 && nums[d - 1] == nums[d])
							d--;
						c++;
						d--;
					}
				}
			}
		}
		return res;
	}
}
```