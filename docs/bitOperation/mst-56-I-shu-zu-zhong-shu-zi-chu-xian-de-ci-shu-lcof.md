# 面试题56-I.数组中数字出现的次数
题目链接：[传送门](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

## 题目描述：
一个整型数组`nums`里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是 $O(n)$，空间复杂度是$O(1)$。

**限制**：$ 2 \leq nums \leq 10000 $

**示例**：

- 输入：nums = `[4,1,4,6]`
- 输出：`[1,6]` 或 `[6,1]`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：首先将所有数进行异或，最后肯定为2个只出现一次的数字进行异或的结果。两个数字不同，进行异或运算的结果肯定有一位为`1`，借用树状数组中的lowbit思想取某个数的二进制最低位表示十进制数，将其分别与数组中每个元素进行与运算分类即可。

## AC代码：
```java
class Solution {
	public int[] singleNumbers(int[] nums) {
		int res = 0, len = nums.length, mark;
		for (int i = 0; i < len; i++)
			res ^= nums[i];
		mark = res & -res; // 取出res的二进制最低位为1表示的十进制数字，注意，该数的二进制其余都是0，只有1个为1，于是我们可以把数组分为2组
		// 另外一个只出现一次的数字肯定跑mark & nums[i] == 0 这组了，而不能与出结果为1这个判断，否则会让更多的数进来导致结果不准确
		int[] ans = new int[2];
		for (int i = 0; i < len; i++) {
			if ((mark & nums[i]) == 0)
				ans[0] ^= nums[i];
			else
				ans[1] ^= nums[i];
		}
		return ans;
	}
}
```
