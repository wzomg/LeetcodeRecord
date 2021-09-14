# 524.通过删除字母匹配到字典里最长单词
题目链接：[传送门](https://leetcode-cn.com/problems/longest-word-in-dictionary-through-deleting/)

## 题目描述：
给你一个字符串`s`和一个字符串数组`dictionary`作为字典，找出并返回字典中最长的字符串，该字符串可以通过删除`s`中的某些字符得到。

如果答案不止一个，返回长度最长且字典序最小的字符串。如果答案不存在，则返回空字符串。

**提示**：
- $1 \leq$ `s.length` $ \leq 1000$
- $1 \leq$ `dictionary.length` $ \leq 1000$
- $1 \leq$ `dictionary[i].length` $\leq 1000$
- `s`和`dictionary[i]`仅由小写英文字母组成

**示例**：
- 输入：`s = "abpcplea", dictionary = ["ale","apple","monkey","plea"]`
- 输出：`"apple"`

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(1)$
- 思路：简单双指针匹配。

## AC代码：
```go
func findLongestWord(s string, dictionary []string) string {
	res := ""
	for _, v := range dictionary {
		i, j := 0, 0
		for j < len(v) && i < len(s) {
			if s[i] == v[j] {
				i, j = i+1, j+1
			} else {
				i++
			}
		}
		if j == len(v) && (len(res) < len(v) || (len(res) == len(v) && res > v)) { //两个字符串的大小可以直接比较
			res = v
		}
	}
	return res
}
```