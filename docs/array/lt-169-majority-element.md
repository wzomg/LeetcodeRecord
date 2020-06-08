# 169.多数元素
题目链接：[传送门](https://leetcode-cn.com/problems/majority-element/)

## 题目描述：
给定一个大小为 $n$ 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于 `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

**示例**:

- 输入: `[2,2,1,1,1,2,2]`
- 输出: `2`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：统计一个数字出现的次数，遇到相同就加1，遇到不同就减一，若计数器为0，就更换当前数字，计数器变为1。因为出现的次数大于n/2下取整，所以照这样下去，最后剩下且至少出现一次的数字就是要求的答案！

## AC代码：
```java
class Solution {
	public int majorityElement(int[] nums) {
		int same = nums[0], cnt = 1, len = nums.length;
		for (int i = 1; i < len; i++) {
			if (nums[i] == same)
				cnt++;
			else {
				cnt--;
				if (cnt == 0) {
					same = nums[i];
					cnt++;
				}
			}
		}
		return same;
	}
}
```