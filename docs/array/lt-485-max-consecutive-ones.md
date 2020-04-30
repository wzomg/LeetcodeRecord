# 485.最大连续1的个数
题目链接：[传送门](https://leetcode-cn.com/problems/max-consecutive-ones/)

## 题目描述：
给定一个二进制数组，计算其中最大连续1的个数。

**注意**：

- 输入的数组只包含`0`和`1`。
- 输入数组的长度是正整数，且不超过`10000`。

**示例**：

输入: `[1,1,0,1,1,1]`
输出: `3`
解释: 开头的两位和最后的三位都是连续`1`，所以最大连续`1`的个数是`3`.

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：简单计数。当前元素不为0就将计数器加1，否则就置为0。

## AC代码：
```java
class Solution {
	public int findMaxConsecutiveOnes(int[] nums) {
		int res = 0, cnt = 0, len;
		if (nums == null || (len = nums.length) == 0)
			return 0;
		for (int i = 0; i < len; i++) {
			if (nums[i] > 0)
				cnt++;
			else
				cnt = 0;
			res = Math.max(res, cnt);
		}
		return res;
	}
}
```
