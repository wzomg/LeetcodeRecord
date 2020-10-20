# 1002.查找常用字符
题目链接：[传送门](https://leetcode-cn.com/problems/find-common-characters/)

## 题目描述：
给定仅有小写字母组成的字符串数组`A`，返回列表中的每个字符串中都显示的全部字符（包括重复字符）组成的列表。例如，如果一个字符在每个字符串中出现`3`次，但不是`4`次，则需要在最终答案中包含该字符`3`次。

你可以按任意顺序返回答案。

## 解决方案：
- 时间复杂度：$O(n \times m)$
- 空间复杂度：$O(26 × n)$
- 思路：简单模拟。判断每个字符串是否含有相同的字符即可。

## AC代码：
```java
class Solution {
	public List<String> commonChars(String[] A) {
		int len;
		List<String> res = new ArrayList<>();
		if (A == null || (len = A.length) == 0)
			return res;
		int[][] cnt = new int[len][26];
		for (int i = 0; i < len; i++)
			for (int j = 0, siz = A[i].length(); j < siz; j++)
				cnt[i][A[i].charAt(j) - 'a']++;
		for (int i = 0, siz = A[0].length(), j; i < siz; i++) {
			for (j = 1; j < len; j++)
				if (cnt[j][A[0].charAt(i) - 'a']-- < 1)
					break;
			if (j == len)
				res.add(String.valueOf(A[0].charAt(i)));
		}
		return res;
	}
}
```