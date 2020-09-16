# 1071.字符串的最大公因子
题目链接：[传送门](https://leetcode-cn.com/problems/greatest-common-divisor-of-strings/)

## 题目描述：
对于字符串`S`和`T`，只有在 $S = T + \cdots + T$（`T` 与自身连接`1`次或多次）时，我们才认定 “T 能除尽 S”。

返回最长字符串`X`，要求满足`X`能除尽`str1`且`X`能除尽`str2`。

**提示**：

- $1 \leq$ `str1.length` $ \leq 1000$
- $1 \leq$ `str2.length` $\leq 1000$
- `str1[i]` 和 `str2[i]` 为大写英文字母

**示例**：

- 输入：`str1 = "ABCABC", str2 = "ABC"`
- 输出：`"ABC"`

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(log^n)$
- 思路：先判断两个字符串拼接之后是否相等，若是则用gcd求其最大公因子，直接截取长度为公因子大小的子串即可！

## AC代码：
```java
class Solution {
	public String gcdOfStrings(String str1, String str2) {
		return !(str1 + str2).equals(str2 + str1) ? "" : str1.substring(0, gcd(str1.length(), str2.length()));
	}
	private int gcd(int a, int b) {
		return b == 0 ? a : gcd(b, a % b);
	}
}
```