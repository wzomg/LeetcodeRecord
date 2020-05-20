# 1371.每个元音包含偶数次的最长子字符串
题目链接：[传送门](https://leetcode-cn.com/problems/find-the-longest-substring-containing-vowels-in-even-counts/)

## 题目描述：
给你一个字符串`s`，请你返回满足以下条件的最长子字符串的长度：每个元音字母，即`a`，`e`，`i`，`o`，`u`，在子字符串中都恰好出现了**偶数次**。

**提示**：

- $ 1 \leq $ `s.length` $ \leq 5 \times 10^5 $
- `s`只包含小写英文字母。

**示例**：

- 输入：`s = "eleetminicoworoep"`
- 输出：`13`
- 解释：最长子字符串是`"leetminicowor"`，它包含`e, i, o`各`2`个，以及`0`个`a, u`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：前缀异或和+状态压缩。将5个元音字母的状态压缩成5个二进制位，标记每个元音字母出现奇数个数为1，偶数个数为0，则状态数一共有 $ 2^5=32 $ 个。采用前缀异或和就能记录每个状态的最初位置，因为要求子串中出现的每个元音字母个数为偶数，对于满足条件的子串而言，两个前缀和`pre[i]`和`pre[j]`的奇偶性一定是相同的，即奇数减奇数等于偶数，偶数减偶数等于偶数。因此，若再次出现相同的二进制数，则更新当前最大值：`res = max(res, i + 1 - pos[num])`，否则标记当前状态首次出现的位置：`pos[num] = i + 1`。注意：异或和为0的下标要初始化为0！

## AC代码：
```java
class Solution {
	public int findTheLongestSubstring(String s) {
		int len, num = 0, res = 0;
		if (s == null || (len = s.length()) == 0)
			return 0;
		int[] pos = new int[1 << 5]; // 32 个状态
		Arrays.fill(pos, -1); // 标记未出现过
		char ch;
		pos[0] = 0;  // 要初始前缀异或和为0的下标为0，不然可能少算几个字符
		for (int i = 0; i < len; i++) {
			ch = s.charAt(i);
			if (ch == 'a')
				num ^= 1 << 0; // 采用异或运算
			else if (ch == 'e')
				num ^= 1 << 1;
			else if (ch == 'i')
				num ^= 1 << 2;
			else if (ch == 'o')
				num ^= 1 << 3;
			else if (ch == 'u')
				num ^= 1 << 4;
			if (pos[num] > -1) // 更新最大值
				res = Math.max(res, i + 1 - pos[num]);
			else // 更新该状态首次出现的位置
				pos[num] = i + 1;
		}
		return res;
	}
}
```