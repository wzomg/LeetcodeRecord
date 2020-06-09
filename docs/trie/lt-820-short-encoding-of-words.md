# 820.单词的压缩编码
题目链接：[传送门](https://leetcode-cn.com/problems/short-encoding-of-words/)

## 题目描述：
给定一个单词列表，我们将这个列表编码成一个索引字符串`S`与一个索引列表`A`。

例如，如果这个列表是`["time", "me", "bell"]`，我们就可以将其表示为`S = "time#bell#"`和 `indexes = [0, 2, 5]`。

对于每一个索引，我们可以通过从字符串`S`中索引的位置开始读取字符串，直到`"#"`结束，来恢复我们之前的单词列表。

那么成功对给定单词列表进行编码的最小字符串长度是多少呢？

**提示**：
- $1 \leq$ `words.length` $ \leq 2000$
- $1 \leq$ `words[i].length` $ \leq 7$
- 每个单词都是小写字母 。

## 解决方案：
- 时间复杂度：$O(n \times m) + O(n \times log^n)$
- 空间复杂度：$O(26^{\max(m_i)+1})$
- 思路：字典树的应用。利用后缀转前缀的特性，先遍历长度较长的字符串，同时倒序遍历把每个字符插入字典树中。若某次插入有开辟新的节点，则说明此字符串不是某个较长字符串的后缀子串，即需要统计这个字符串的长度+1。

## AC代码：
```java
class Solution {
	private class TrieNode {
		private TrieNode[] next;
		public TrieNode() {
			next = new TrieNode[26];
		}
	}
    // 定义一个全局的 root
	private TrieNode root; 
	public int minimumLengthEncoding(String[] words) {
		int len, ans = 0;
		if (words == null || (len = words.length) == 0)
			return 0;
		root = new TrieNode();
        // 字符串长度从小到大排序
		Arrays.sort(words, new Comparator<String>() {
			@Override
			public int compare(String o1, String o2) {
				return o1.length() - o2.length();
			}
		});
		for (int i = len - 1; i >= 0; i--)
			ans += insert(words[i]);
		return ans;
	}
	private int insert(String str) {
		TrieNode cur = root;
		boolean flag = false;
        // 倒序遍历该字符串中的每个字符并依次插入字典树中
		for (int i = str.length() - 1, ch; i >= 0; i--) {
			ch = str.charAt(i) - 'a';
            // 只要开辟了一个新的节点，则说明此字符换肯定不是一个后缀子串
			if (cur.next[ch] == null) { 
				flag = true;
				cur.next[ch] = new TrieNode();
			}
			cur = cur.next[ch];
		}
		return flag ? str.length() + 1 : 0;
	}
}
```