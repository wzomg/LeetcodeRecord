# 165.比较版本号
题目链接：[传送门](https://leetcode-cn.com/problems/compare-version-numbers/)

## 题目描述：
给你两个版本号version1和version2，请你比较它们。

版本号由一个或多个修订号组成，各修订号由一个`'.'`连接。每个修订号由**多位数字**组成，可能包含前导零 。每个版本号至少包含一个字符。修订号从左到右编号，下标从0开始，最左边的修订号下标为0，下一个修订号下标为 1 ，以此类推。例如，2.5.33 和 0.1 都是有效的版本号。

比较版本号时，请按从左到右的顺序依次比较它们的修订号。比较修订号时，只需比较**忽略任何前导零后的整数值**。也就是说，修订号1和修订号001相等 。如果版本号没有指定某个下标处的修订号，则该修订号视为0。例如，版本 1.0 小于版本 1.1 ，因为它们下标为0的修订号相同，而下标为1的修订号分别为 0 和 1 ，0 < 1 。

返回规则如下：
- 如果 version1 > version2 返回 1，
- 如果 version1 < version2 返回 -1，
- 除此之外返回 0。

**提示**：
- $1 \leq $ `version1.length, version2.length` $ \leq 500$
- version1 和 version2 仅包含数字和 '.'
- version1 和 version2 都是 有效版本号
- version1 和 version2 的所有修订号都可以存储在 32 位整数中

**示例**：
- 输入：version1 = "1.01", version2 = "1.001"
- 输出：0
- 解释：忽略前导零，"01" 和 "001" 都表示相同的整数 "1"

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：双指针模拟即可。

## AC代码：
```go
func compareVersion(version1 string, version2 string) int {
	i, j, len1, len2, num1, num2 := 0, 0, len(version1), len(version2), 0, 0
	for i < len1 || j < len2 {
		num1 = 0
		num2 = 0
		for ; i < len1 && version1[i] != '.'; i++ {
			num1 = num1*10 + int(version1[i]-'0')
		}
		for ; j < len2 && version2[j] != '.'; j++ {
			num2 = num2*10 + int(version2[j]-'0')
		}
		if num1 < num2 {
			return -1
		}
		if num1 > num2 {
			return 1
		}
		i++
		j++
	}
	return 0
}
```