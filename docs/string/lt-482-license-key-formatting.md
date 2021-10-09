# 482.密钥格式化
题目链接：[传送门](https://leetcode-cn.com/problems/license-key-formatting/)

## 题目描述：
有一个密钥字符串S，只包含字母，数字以及`'-'`（破折号）。其中，N个`'-'`将字符串分成了$N+1$组。

给你一个数字K，请你重新格式化字符串，使每个分组恰好包含K个字符。特别地，第一个分组包含的字符个数必须小于等于K，但至少要包含1个字符。两个分组之间需要用`'-'`（破折号）隔开，并且将所有的小写字母转换为大写字母。

给定非空字符串S和数字K，按照上面描述的规则进行格式化。

**提示**：
- S 的长度可能很长，请按需分配大小。K 为正整数。
- S 只包含字母数字（`a-z`，`A-Z`，`0-9`）以及破折号'-'
- S 非空

**示例**：
- 输入：`S = "2-5g-3-J", K = 2`
- 输出：`"2-5G-3J"`
- 解释：字符串 S 被分成了 3 个部分，按照前面的规则描述，第一部分的字符可以少于给定的数量，其余部分皆为 2 个字符。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：简单模拟即可。

## AC代码：
```go
func licenseKeyFormatting(s string, k int) string {
	s = strings.ReplaceAll(s, "-", "")
	Ls := len(s)
	rs := Ls % k
	res := strings.Builder{}
	res.WriteString(s[:rs])
	for i := rs; i < Ls; i += k {
		if res.Len() > 0 {
			res.WriteByte('-')
		}
		res.WriteString(s[i : i+k])
	}
	return strings.ToUpper(res.String())
}
```