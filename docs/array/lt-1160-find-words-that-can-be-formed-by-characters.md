# 1160.拼写单词
题目链接：[传送门](https://leetcode-cn.com/problems/find-words-that-can-be-formed-by-characters/)

## 题目描述：
给你一份『词汇表』（字符串数组）`words`和一张『字母表』（字符串）`chars`。

假如你可以用`chars`中的『字母』（字符）拼写出`words`中的某个『单词』（字符串），那么我们就认为你掌握了这个单词。

注意：每次拼写（指拼写词汇表中的一个单词）时，`chars`中的每个字母都只能用一次。

返回词汇表`words`中你掌握的所有单词的**长度之和**。

**提示**：
- $1 \leq$ `words.length` $ \leq 1000$
- $1 \leq$ `words[i].length, chars.length` $ \leq 100$
- 所有字符串中都仅包含小写英文字母

## 解决方案：
- 时间复杂度：$O(m \times n)$
- 空间复杂度：$O(m \times n)$
- 思路：先统计`chars`字符串中的每个字符出现的个数，再遍历其是否有足够的字符组成words中的每个字符串，若是，则累加符合条件的字符串长度即可！

## AC代码：
```java
class Solution {
	public int countCharacters(String[] words, String chars) {
		int res = 0, lenW, lenS;
		if (words == null || (lenW = words.length) == 0 || chars == null || (lenS = chars.length()) == 0)
			return 0;
		int[] cnt = new int[26];
		int[] nums = new int[26];
		for (int i = 0; i < lenS; i++)
			cnt[chars.charAt(i) - 'a']++;
		boolean flag;
		for (int i = 0, lenT; i < lenW; i++) {
			lenT = words[i].length();
			flag = true;
			// 系统级别的native原生方法，效率高。
			// 参数含义是：
			// （原数组， 原数组的开始位置， 目标数组， 目标数组的开始位置， 拷贝个数）
			System.arraycopy(cnt, 0, nums, 0, 26);
			for (int j = 0; j < lenT && flag; j++)
				if (nums[words[i].charAt(j) - 'a']-- == 0)
					flag = false;
			if (flag)
				res += lenT;
		}
		return res;
	}
}
```