# 405.数字转换为十六进制数
题目链接：[传送门](https://leetcode-cn.com/problems/convert-a-number-to-hexadecimal/)

## 题目描述：
给定一个整数，编写一个算法将这个数转换为十六进制数。对于负整数，我们通常使用**补码运算**方法。

**注意**：
- 十六进制中所有字母(a-f)都必须是小写。
- 十六进制字符串中不能包含多余的前导零。如果要转化的数为0，那么以单个字符'0'来表示；对于其他情况，十六进制字符串中的第一个字符将不会是0字符。 
- 给定的数确保在32位有符号整数范围内。
- 不能使用任何由库提供的将数字直接转换或格式化为十六进制的方法。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：位运算。从高位到低位每4位与`0xf`进行与运算即可。

## AC代码：
```go
func toHex(num int) string {
	res := strings.Builder{}
	for i := 7; i >= 0; i-- { // 从高位往低位遍历
		cur := num >> (4 * i) & 0xf
		if cur > 9 {
			res.WriteByte(byte(cur-10) + 'a')
		} else {
			res.WriteByte(byte(cur) + '0')
		}
	}
	sb := res.String()
	pos := 0
	for ; pos < len(sb) && sb[pos] == '0'; pos++ { //去除前缀0
	}
	if pos == len(sb) {
		return "0"
	}
	return sb[pos:]
}
```