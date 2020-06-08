# 409.最长回文串
题目链接：[传送门](https://leetcode-cn.com/problems/longest-palindrome/)

## 题目描述：
给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。

在构造过程中，请注意区分大小写。比如 "Aa" 不能当做一个回文字符串。

**注意**：假设字符串的长度不会超过 $1010$。

**示例**:
- 输入：`"abccccdd"`
- 输出：`7`
- 解释：我们可以构造的最长的回文串是`"dccaccd"`, 它的长度是`7`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：贪心取即可！

## AC代码：
```java
class Solution {
	public int longestPalindrome(String s) {
		int len, res = 0;
		if (s == null || (len = s.length()) == 0)
			return 0;
		int[] cnt = new int[64];
		boolean hasOne = false;
		for (int i = 0; i < len; i++)
			cnt[s.charAt(i) - 'A']++;
		for (int i = 0; i < 64; i++) {
			if (cnt[i] > 0) {
				res += cnt[i] / 2 * 2;
				if ((cnt[i] & 1) == 1)
					hasOne = true;
			}
		}
		if (hasOne)
			res++;
		return res;
	}
}
```