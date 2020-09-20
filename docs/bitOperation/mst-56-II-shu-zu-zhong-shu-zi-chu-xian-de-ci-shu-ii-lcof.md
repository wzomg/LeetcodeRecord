# 面试题56-II.数组中数字出现的次数II
题目链接：[传送门](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

## 题目描述：
在一个数组`nums`中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。

**限制**：

- $1 \leq $ `nums.length` $ \leq 10000$
- $1 \leq$ `nums[i]` $ < 2^{31} $

**示例**：

- 输入：`nums = [3,4,3,3]`
- 输出：`4`

## 解决方案：
- 时间复杂度：$O(32 \times n)$
- 空间复杂度：$O(1)$
- 思路：统计每个数二进制的每位bit总和，若最后不能取余3的，说明这个位对答案有贡献！
- 备注：酷家乐笔试题。

## AC代码：
```java
class Solution {
	public int singleNumber(int[] nums) {
		int[] bits = new int[32];
		// 给出32位是因为最高位可能是1，也就是代表负数
		for (int i = 0, len = nums.length; i < len; i++) {
			for (int j = 0; j < 32; j++) // 使用右移动避免数据溢出
				bits[j] += ((nums[i] >> j) & 1);
		}
		int res = 0;
		// 2147483647为2^{31}-1，也就是31个1，最高位为0，不会溢出
		for (int i = 0; i < 32; i++)
			if (bits[i] % 3 != 0)
				res |= 1 << i;
		return res;
	}
}
```