# 443.压缩字符串
题目链接：[传送门](https://leetcode-cn.com/problems/string-compression/)

## 题目描述：
给你一个字符数组chars，请使用下述算法压缩：

从一个空字符串s开始。对于chars中的每组连续重复字符 ：

如果这一组长度为1，则将字符追加到s中。否则，需要向s追加字符，后跟这一组的长度。

压缩后得到的字符串 s 不应该直接返回 ，需要转储到字符数组chars中。需要注意的是，如果组长度为10或10以上，则在 chars 数组中会被拆分为多个字符。

请在**修改完输入数组后**，返回该数组的新长度。

你必须设计并实现一个只使用常量额外空间的算法来解决此问题。

**提示**：
- $ 1 \leq $ `chars.length` $ \leq 2000$
- `chars[i]`可以是小写英文字母、大写英文字母、数字或符号

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：简单模拟即可。

## AC代码：
```go
func compress(chars []byte) int {
	lenC := len(chars)
	if lenC < 1 {
		return 0
	}
	curCh, cnt, idx := chars[0], 1, 0
	for i := 1; i < lenC; i++ {
		if chars[i] == curCh {
			cnt++
		} else {
			chars[idx] = curCh
			idx++
			if cnt != 1 {
				str := strconv.Itoa(cnt) // 数字转字符串
				for _, v := range str {
					chars[idx] = byte(v)
					idx++
				}
			}
			curCh = chars[i]
			cnt = 1
		}
	}
	chars[idx] = curCh
	idx++
	if cnt != 1 {
		str := strconv.Itoa(cnt)
		for _, v := range str {
			chars[idx] = byte(v)
			idx++
		}
	}
	return idx
}
```