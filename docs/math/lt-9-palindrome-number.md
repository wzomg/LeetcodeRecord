# 9.回文数
题目链接：[传送门](https://leetcode-cn.com/problems/palindrome-number/)

## 题目描述：
判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

**进阶**：你能不将整数转为字符串来解决这个问题吗？

**示例**：
- 输入: `121`
- 输出: `true`

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(1)$
- 思路：反转整个数字，可能会造成数据溢出；而反转一半数字，只需要筛去一些不符合条件的数字并判断这两个数字是否相等即可。

## AC代码：
```java
class Solution {
	public boolean isPalindrome(int x) {
		// 原数不能是负数，或是10的倍数（除0）
		if (x < 0 || (x % 10 == 0 && x != 0))
			return false;
		int y = 0;
		while (y < x) {
			y = y * 10 + x % 10;
			x /= 10;
		}
        // 偶数位、奇数位
		return x == y || x == y / 10;
	}
}
```