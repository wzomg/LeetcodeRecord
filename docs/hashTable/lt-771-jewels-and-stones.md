# 771.宝石与石头
题目链接：[传送门](https://leetcode-cn.com/problems/jewels-and-stones/)

## 题目描述：
给定字符串`J`代表石头中宝石的类型，和字符串`S`代表你拥有的石头。`S`中每个字符代表了一种你拥有的石头的类型，你想知道你拥有的石头中有多少是宝石。

`J`中的字母不重复，`J`和`S`中的所有字符都是字母。字母区分大小写，因此`"a"`和`"A"`是不同类型的石头。

**注意**：

- `S`和`J`最多含有`50`个字母。
- `J`中的字符不重复。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：哈希表。标记J中的每个字符，统计S字符串中出现的个数即可。

## AC代码：
```java
class Solution {
	public int numJewelsInStones(String J, String S) {
		int res = 0, len1, len2;
		if (J == null || (len1 = J.length()) == 0 || S == null || (len2 = S.length()) == 0)
			return 0;
		boolean[] vis = new boolean[256];
		for (int i = 0; i < len1; i++)
			vis[J.charAt(i)] = true;
		for (int i = 0; i < len2; i++)
			if (vis[S.charAt(i)])
				res++;
		return res;
	}
}
```