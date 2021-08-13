# 233.数字1的个数
题目链接：[传送门](https://leetcode-cn.com/problems/number-of-digit-one/)

## 题目描述：
给定一个整数`n`，计算所有小于等于`n`的非负整数中数字`1`出现的个数。

**提示**：$ 0 \leq n \leq 2 \times 10^9 $

**示例**：
- 输入：n = 13
- 输出：6

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(1)$
- 思路：数学+找规律。分别拿1204中十位数是0，1或2~9来讨论并找规律：

```
1204：十位上的数字为1 -> 0010~1119（最大范围），锁定十位即不看十位 -> 000~119 -> 120个数字 -> 12 * 10 -> high * digit
1214：十位上的数字为1 -> 0010~1214（最大范围），锁定十位即不看十位 -> 000~124 -> 125个数字 -> 12 * 10 + 4 + 1 -> high * digit + low + 1
1234：十位上的数字为1 -> 0010~1219（最大范围），锁定十位即不看十位 -> 000~129 -> 130个数字 -> (12 + 1) * 10 -> (high + 1) * digit
```

## AC代码：
```go
func countDigitOne(n int) int {
	if n < 1 {
		return 0
	}
	//digit：表示在第几位，个位、十位...
	//res：计算结果
	//high：高位数字
	//cur：当前位
	//low：低位数字
	digit, res, high, cur, low := 1, 0, n/10, n%10, 0
	for high != 0 || cur != 0 {
		if cur == 0 {
			res += high * digit
		} else if cur == 1 {
			res += high*digit + low + 1
		} else {
			res += (high + 1) * digit
		}
		low += cur * digit
		cur = high % 10
		high /= 10
		digit *= 10
	}
	return res
}
```