# 151.翻转字符串里的单词
题目链接：[传送门](https://leetcode-cn.com/problems/reverse-words-in-a-string/)

## 题目描述：
给定一个字符串，逐个翻转字符串中的每个单词。

**示例**：

- 输入: `"the sky is blue"`
- 输出: `"blue is sky the"`

**说明**：

- 无空格字符构成一个单词。
- 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
- 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

**进阶**：

请选用`C`语言的用户尝试使用$O(1)$额外空间复杂度的原地解法。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：先切割字符串，再逆序遍历添加即可。

> 个人的碎碎念：第一次腾讯二面时，面试官就给出这道题，当时老问我空间复杂度可否优化为$O(1)$，我一个用`java`刷题怎么可能实现。。。`java`的`String`类中存储字符串中的字符是这样定义的：`private final char value[];`，`final`修饰的`char`数组不能直接修改字符串本身！何况题目中**进阶**已经说清楚了，用`C`语言可以实现时间换空间，投的是`java`岗面试官也不知道嘛？？？真是醉了。。。

## AC代码：
```java
class Solution {
	public String reverseWords(String s) {
		int len;
		if (s == null || (len = s.length()) == 0)
			return s;
		// 先按照空格切分字符串
		String[] strs = s.split(" ");
		StringBuilder sb = new StringBuilder();
		for (int i = strs.length - 1; i >= 0; i--) {
			// 相邻空格切分出来的字符串的长度为0，故需要特判
			if (strs[i].length() != 0)
				sb.append(strs[i]).append(" ");
		}
		if (sb.length() > 0) // 删除末尾多余的空格
			sb.deleteCharAt(sb.length() - 1);
		return sb.toString();
	}
}
```