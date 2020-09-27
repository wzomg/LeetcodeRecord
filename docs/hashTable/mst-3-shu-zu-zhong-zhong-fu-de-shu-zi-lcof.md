# 面试题3.数组中重复的数字
题目链接：[传送门](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

## 题目描述：
找出数组中重复的数字。

在一个长度为`n`的数组`nums`里的所有数字都在 $0 \sim n-1$ 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

**限制**：$ 2 \leq n \leq 100000$

**示例**：

- 输入：`[2, 3, 1, 0, 2, 5, 3]`
- 输出：`2`或`3` 

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：使用HashSet是无序不重复的集合即可解决！

## AC代码：
```java
class Solution {
	public int findRepeatNumber(int[] nums) {
		Set<Integer> set = new HashSet<Integer>();
		int res = -1;
		for (int i = 0, len = nums.length; i < len; i++) {
            // 若添加失败，则说明当前元素重复了
			if (!set.add(nums[i])) { 
				res = nums[i];
				break;
			}
		}
		return res;
	}
}
```