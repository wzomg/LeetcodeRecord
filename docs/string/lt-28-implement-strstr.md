# 28.实现strStr()
题目链接：[传送门](https://leetcode-cn.com/problems/implement-strstr/)

## 题目描述：
实现 strStr() 函数。

给定一个`haystack`字符串和一个`needle`字符串，在`haystack`字符串中找出`needle`字符串出现的第一个位置 (从0开始)。如果不存在，则返回`-1`。

**说明**:

当`needle`是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当`needle`是空字符串时我们应当返回`0`。这与C语言的`strstr()`以及`Java`的`indexOf()`定义相符。

**示例**:

- 输入: `haystack = "hello"`, `needle = "ll"`
- 输出: `2`

## 解决方案：
- 时间复杂度：$O(m + n)$
- 空间复杂度：$O(n)$
- 思路：KMP算法找文本串中是否有匹配的模式串。

## AC代码：
```java
class Solution {
	public int strStr(String text, String pattern) {
		int lenT, lenP, j = 0, i = 0, rt = -1; // 记得都是从第一个字符开始匹配
		if (text == null || pattern == null)
			return -1;
		lenT = text.length();
		lenP = pattern.length();
		if ((lenT == 0 && lenP == 0) || lenP == 0)
			return 0;
		int[] prefix = new int[lenP + 1]; // !
		get_prefix_table(prefix, pattern);
		while (i < lenT) {
			if (j == -1 || text.charAt(i) == pattern.charAt(j)) {
				i++;
				j++;
			} else
				j = prefix[j];
			if (j == lenP) {
				rt = i - lenP;
				break;
			}
		}
		return rt;
	}
	// 获得模式串的前缀表
	private void get_prefix_table(int[] prefix, String pattern) {
		int pos = -1, j = 0, lenP = pattern.length();
		prefix[0] = -1; // 记得初始值为-1
		while (j < lenP) {
			if (pos == -1 || pattern.charAt(pos) == pattern.charAt(j))
				prefix[++j] = ++pos;
			else
				pos = prefix[pos];
		}
	}
}
```
