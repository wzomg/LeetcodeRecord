# 76.最小覆盖子串
题目链接：[传送门](https://leetcode-cn.com/problems/minimum-window-substring/)

## 题目描述：
给你一个字符串`S`、一个字符串`T`，请在字符串`S`里面找出：包含`T`所有字符的最小子串。

**说明**：

- 如果`S`中不存这样的子串，则返回空字符串`""`。
- 如果`S`中存在这样的子串，我们保证它是唯一的答案。

**示例**：

- 输入: `S = "ADOBECODEBANC"`, `T = "ABC"`
- 输出: `"BANC"`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：尺取法。两个指针，一个关键点：如何统计S字符串某个区间是否都包含字符串T呢？只需通过种类数来判断，即若该区间中每个字母都达到了T字符串中该种字母的数量，种类数才加1。

## AC代码：
```java
class Solution {
	public String minWindow(String s, String t) {
		int st = 0, ed = 0, lenS, lenT, kind = 0, curK = 0, begin = 0, minL;
		if (s == null || t == null || (lenS = s.length()) == 0 || (lenT = t.length()) == 0 || lenS < lenT)
			return "";
		int[] cnt = new int[128]; // 'z'的ASCII为122，那么取个2的幂次值，只要大于122都可取
		int[] num = new int[128];
		for (int i = 0; i < lenT; i++)
			if (cnt[t.charAt(i)]++ == 0)
				kind++; // 统计总类数
		char ch;
		minL = lenS + 1; // minL为最小区间长度，初始值应该为lenS+1，若minL值没有变化，有可能是"a" "b"
		while (true) {
			while (ed < lenS && curK < kind) {
				ch = s.charAt(ed);
				if (++num[ch] == cnt[ch]) // 如果某个字母刚好已达到自己的数量，那么种类个数加1
					curK++;
				ed++; // 右移指针
			}
			if (curK < kind) // 如果已经没有凑齐的了， 就直接退出
				break;
			// 此时curK == Kind
			if (ed - st < minL) {
				begin = st;
				minL = ed - st; // 左闭右开区间[st, ed)，区间个数直接相减
			}
			ch = s.charAt(st);
			if (num[ch]-- == cnt[ch]) // 先判断一下个数减1前是否刚好达到自己的数量，是则将种类数减1
				curK--;
			st++; // 右移左指针
		}
		return minL == lenS + 1 ? "" : s.substring(begin, begin + minL);
	}
}
```