# 914.卡牌分组
题目链接：[传送门](https://leetcode-cn.com/problems/x-of-a-kind-in-a-deck-of-cards/)

## 题目描述：
给定一副牌，每张牌上都写着一个整数。

此时，你需要选定一个数字`X`，使我们可以将整副牌按下述规则分成 $1$ 组或更多组：

- 每组都有`X`张牌。
- 组内所有的牌上都写着相同的整数。

仅当你可选的 $X \geq 2$ 时返回`true`。

**提示**：
- $1 \leq $ `deck.length` $ \leq 10000$
- $0 \leq $ `deck[i]` $< 10000$

**示例1**：
- 输入：`[1,2,3,4,4,3,2,1]`
- 输出：`true`
- 解释：可行的分组是`[1,1], [2,2], [3,3], [4,4]`

**示例2**：
- 输入：`[1,1,2,2,2,2]`
- 输出：`true`
- 解释：可行的分组是`[1,1], [2,2], [2,2]`

## 解决方案：
- 时间复杂度：$O(n × log^n)$
- 空间复杂度：$O(n)$
- 思路：简单数学。计算出不同种类数间的最大公约数`X`即可。

## AC代码：
```java
class Solution {
	public boolean hasGroupsSizeX(int[] deck) {
		int len, X;
		if (deck == null || (X = len = deck.length) == 0)
			return false;
		int[] cnt = new int[10001];
		boolean flag = true;
		for (int i = 0; i < len; i++)
			cnt[deck[i]]++;
		for (int i = 0; i <= 10000; i++)
			if (cnt[i] > 0) {
				if (flag) {
					X = cnt[i];
					flag = false;
				} else
					X = gcd(X, cnt[i]);
			}
		return X > 1;
	}
	private int gcd(int a, int b) {
		return b == 0 ? a : gcd(b, a % b);
	}
}
```