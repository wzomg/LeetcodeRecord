# 397.整数替换
题目链接：[传送门](https://leetcode-cn.com/problems/integer-replacement/)

## 题目描述：
给定一个正整数`n`，你可以做如下操作：

1. 如果`n`是偶数，则用`n / 2`替换`n`。
2. 如果`n`是奇数，则可以用`n + 1`或`n - 1`替换`n`。

`n`变为`1`所需的最小替换次数是多少？

**示例**：

- 输入：`7`
- 输出：`4`
- 解释：`7 -> 8 -> 4 -> 2 -> 1`或`7 -> 6 -> 3 -> 2 -> 1`

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(log^n)$
- 思路：暴搜或者位运算。位运算思路：找规律。当 $ n > 3 \land \; n \mod 2 == 1 $ 时，看n的最低2位，这2位只有2种情况：`11`和`01`，若是`01`则`-1`；若是`11`，则`+1`之后会出现2个0，通过找规律可以发现采用`+1`比`-1`使得替换的次数少，故最低2位是`11`时，使用`+1`。目的是每次操作让其二进制位1的数目尽量少。另外要特判`n=3`要`-1`，虽然其最低2位是`11`，但是`-1`比`+1`要少一次替换。

## AC代码1：暴搜
```java
class Solution {
	public int integerReplacement(int n) {
		return (int) dfs(n);
	}
	private long dfs(long s) {
		if (s == 1)
			return 0;
		long res = 0;
		if (s % 2 == 0)
			res = dfs(s >> 1) + 1;
		else
			res = Math.min(dfs(s + 1), dfs(s - 1)) + 1;
		return res;
	}
}
```

## AC代码2：位运算
```java
class Solution {
	public int integerReplacement(int n) {
		int cnt = 0;
		if (n == Integer.MAX_VALUE) // 特判
			return 32;
		while (n != 1) {
			if ((n & 1) == 0) // 若是偶数则直接右移即可
				n >>= 1;
			else // 否则若其二进制最低2位为 01 或者 n == 3，则使用 n-1，其余情况都使用 n+1
				n += ((n & 2) == 0 || n == 3) ? -1 : 1;
			cnt++; // 替换次数加1
		}
		return cnt;
	}
}
```