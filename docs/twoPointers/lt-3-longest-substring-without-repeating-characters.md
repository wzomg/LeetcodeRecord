# 3.无重复字符的最长子串
题目链接：[传送门](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

## 题目描述：
给定一个字符串，请你找出其中不含有重复字符的**最长子串**的长度。

**示例**:

- 输入: `"abcabcbb"`
- 输出: `3`
- 解释: 因为无重复字符的最长子串是`"abc"`，所以其长度为`3`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：以右指针为参照点，若当前右指针指向字符出现的次数大于1，则不断地移动左指针（不超过右指针），然后更新一下区间元素不重复出现的最多个数：`max(res,rt−lt+1)`即可！

## AC代码：
```java
class Solution {
	public int lengthOfLongestSubstring(String s) {
		int len, lt = 0, rt = 0, res = 0;
		if (s == null || (len = s.length()) == 0)
			return 0;
		int[] cnt = new int[256];
		while (rt < len) {
			cnt[s.charAt(rt)]++;
			while (lt < rt && cnt[s.charAt(rt)] > 1) {
				cnt[s.charAt(lt)]--;
				lt++;
			}
			res = Math.max(res, rt - lt + 1);
			rt++;
		}
		return res;
	}
}
```