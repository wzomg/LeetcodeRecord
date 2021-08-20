# 541.反转字符串II
题目链接：[传送门](https://leetcode-cn.com/problems/reverse-string-ii/)

## 题目描述：
给定一个字符串s和一个整数k，从字符串开头算起，每2k个字符反转前k个字符。

如果剩余字符少于 k 个，则将剩余字符全部反转。

如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样。

**提示**：

- $1 \leq $ `s.length` $ \leq 10^4$
- s仅由小写英文组成
- $ 1 \leq k \leq 10^4$

**示例**：
- 输入：s = "abcdefg", k = 2
- 输出："bacdfeg"

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：简单模拟。

## AC代码：
```go
func reverseStr(s string, k int) string {
	i, step := 0, 2*k
	str := []byte(s)
	lenS := len(s)
	lenT := lenS - lenS%step
	for ; i < lenT; i += step {
		reverse(i, i+k-1, str)
	}
	lenR := lenS - lenT //剩下字符串的长度
	if lenR <= k {
		reverse(lenT, lenS-1, str)
	} else {
		reverse(lenT, lenT+k-1, str)
	}
	return string(str)
}
func reverse(st, ed int, arr []byte) {
	for ; st < ed; st, ed = st+1, ed-1 {
		arr[st], arr[ed] = arr[ed], arr[st]
	}
}
```