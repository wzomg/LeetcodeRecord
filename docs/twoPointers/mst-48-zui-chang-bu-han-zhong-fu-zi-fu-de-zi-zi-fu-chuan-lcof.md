# 面试题48.最长不含重复字符的子字符串
题目链接：[传送门]()

## 题目描述：
请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

**提示**：`s.length <= 40000`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：滑动窗口求区间不同字符的最大个数。宗旨是固定右指针，注意是固定右指针，每次检查右指针对应字符在该区间`[lt,rt]`出现的个数是否大于1，若是则先将左指针对应字符出现的个数减1，然后向前移动直到满足该区间中所有数都不同才统计答案，此时右指针向右移动一个单位，直到遍历完该字符串为止！

## AC代码：
```java
class Solution {
	public int lengthOfLongestSubstring(String s) {
		int len, res = 0, lt = 0, rt = 0;
		if (s == null || (len = s.length()) == 0)
			return 0;
		int[] nums = new int[256];
		while (rt < len) {
			nums[s.charAt(rt)]++;
			while (lt < rt && nums[s.charAt(rt)] > 1)
				nums[s.charAt(lt++)]--;
			res = Math.max(res, rt - lt + 1);
			rt++;
		}
		return res;
	}
}
```
