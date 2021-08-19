# 345.反转字符串中的元音字母
题目链接：[传送门](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/)

## 题目描述：
编写一个函数，以字符串作为输入，反转该字符串中的元音字母。

**提示**：元音字母不包含字母 "y" 。

**示例**：
- 输入："hello"
- 输出："holle"

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：简单双指针+哈希。

## AC代码：
```go
func reverseVowels(s string) string {
	vowels := map[byte]bool{'a': true, 'e': true, 'i': true, 'o': true, 'u': true,
		'A': true, 'E': true, 'I': true, 'O': true, 'U': true}
	str := []byte(s)                  //字符串转切片才能进行修改
	for i, j := 0, len(s)-1; i < j; { //双指针
		_, ok1 := vowels[str[i]]
		_, ok2 := vowels[str[j]]
		if !ok1 {
			i++
			continue
		}
		if !ok2 {
			j--
			continue
		}
		swap(i, j, str)
		i, j = i+1, j-1
	}
	return string(str) //将切片转为字符串
}
func swap(i, j int, str []byte) {
	if i == j || str[i] == str[j] {
		return
	}
	str[i] ^= str[j]
	str[j] ^= str[i]
	str[i] ^= str[j]
}
```