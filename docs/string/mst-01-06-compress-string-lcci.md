# 面试题01.06.字符串压缩
题目链接：[传送门](https://leetcode-cn.com/problems/compress-string-lcci/)

## 题目描述：
字符串压缩。利用字符重复出现的次数，编写一种方法，实现基本的字符串压缩功能。比如，字符串`aabcccccaaa`会变为`a2b1c5a3`。若“压缩”后的字符串没有变短，则返回原先的字符串。你可以假设字符串中只包含大小写英文字母（`a`至`z`）。

**提示**：字符串长度在`[0, 50000]`范围内。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：简单模拟。

## AC代码：
```java
class Solution {
	public String compressString(String S) {
		int len;
		if (S == null || (len = S.length()) == 0)
			return "";
		StringBuilder sb = new StringBuilder();
		char pre = S.charAt(0);
		int cnt = 1;
		for (int i = 1; i < len; i++) {
			if (S.charAt(i) == pre)
				cnt++;
			else {
				sb.append(pre + String.valueOf(cnt));
				pre = S.charAt(i);
				cnt = 1;
			}
		}
		sb.append(pre + String.valueOf(cnt));
		return sb.length() < len ? sb.toString() : S;
	}
}
```