# 139.单词拆分
题目链接：[传送门](https://leetcode-cn.com/problems/word-break/)

## 题目描述：
给定一个非空字符串`s`和一个包含非空单词列表的字典`wordDict`，判定`s`是否可以被空格拆分为一个或多个在字典中出现的单词。

**说明**：

- 拆分时可以重复使用字典中的单词。
- 你可以假设字典中没有重复的单词。

**示例**：

- 输入: `s = "applepenapple"`, `wordDict = ["apple", "pen"]`
- 输出: `true`
- 解释: 返回`true`因为`"applepenapple"`可以被拆分成`"apple pen apple"`。注意你可以重复使用字典中的单词。

## 解决方案：
- 时间复杂度：$O(n^2)$，每个起点都会遍历到末尾
- 空间复杂度：$O(n)$，栈最深为n
- 思路：记忆化搜索，我们以每个点作为起点，遍历其后面的位置构成当前字符串的子串，若子串包含在字典中，则继续从end位置递归查找下去，这样会导致大量重复计算子问题，做一个记忆化即可。这里记忆化可用哈希map，也可以用boolean的包装类Boolean！

## AC代码：
```java
class Solution {
	private Set<String> set;
	private Boolean[] isT;
	public boolean wordBreak(String s, List<String> wordDict) {
		int len = s.length();
		set = new HashSet<String>(wordDict);
		isT = new Boolean[len]; // 使用包装类
		return dfs(0, s, len);
	}
	private boolean dfs(int st, String s, int len) {
		if (st == len) // 终止条件
			return true;
		if (isT[st] != null) // 记忆化子问题
			return isT[st];
		for (int ed = st + 1; ed <= len; ed++) { // 左闭右开区间
			if (set.contains(s.substring(st, ed)) && dfs(ed, s, len))
				return isT[st] = true; // 记忆化子问题
		}
		return isT[st] = false;
	}
}
```