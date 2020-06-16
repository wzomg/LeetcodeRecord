# 14.最长公共前缀
题目链接：[传送门](https://leetcode-cn.com/problems/longest-common-prefix/)

## 题目描述：
编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串`""`。

**说明**：所有输入只包含小写字母 $a-z$ 。

## 解决方案：
- 时间复杂度：$O(n \times m)$
- 空间复杂度：$O(n)$
- 思路：简单字符串操作，每次保存前i个字符串的公共前缀子串即可。

## AC代码：
```java
class Solution {
	public String longestCommonPrefix(String[] strs) {
		int len;
		if (strs == null || (len = strs.length) == 0)
			return "";
		String pre = strs[0];
		for (int i = 1, cur, l1, l2, lmin; i < len; i++) {
			cur = 0;
			l1 = pre.length();
			l2 = strs[i].length();
			lmin = Math.min(l1, l2);
			while (cur < lmin && pre.charAt(cur) == strs[i].charAt(cur))
				cur++;
			pre = strs[i].substring(0, cur);
		}
		return pre;
	}
}
```