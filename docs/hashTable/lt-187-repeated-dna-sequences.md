# 187.重复的DNA序列
题目链接：[传送门](https://leetcode-cn.com/problems/repeated-dna-sequences/)

## 题目描述：
所有DNA都由一系列缩写为`'A'`，`'C'`，`'G'`和`'T'`的核苷酸组成，例如：`"ACGAATTCCG"`。在研究 DNA时，识别DNA中的重复序列有时会对研究非常有帮助。

编写一个函数来找出所有目标子串，目标子串的长度为10，且在`DNA`字符串`s`中出现次数超过一次。

**提示**：
- $ 0 \leq $ `s.length` $ \leq 10^5$
- `s[i]`为`'A'`、`'C'`、`'G'`或`'T'`

**示例**：
- 输入：`s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"`
- 输出：`["AAAAACCCCC","CCCCCAAAAA"]`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：遍历一次取出所有长度为10的子串并放入哈希表中，再遍历一次哈希表取出出现次数大于1的key即可。

## AC代码：
```go
func findRepeatedDnaSequences(s string) []string {
	lenS := len(s)
	if lenS < 10 {
		return []string{}
	}
	cnt := make(map[string]int, lenS-9)
	res := make([]string, 0)
	for i := 0; i < lenS-9; i++ {
		cnt[s[i:i+10]]++
	}
	for k, v := range cnt {
		if v > 1 {
			res = append(res, k)
		}
	}
	return res
}
```