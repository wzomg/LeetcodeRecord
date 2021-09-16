# 212.单词搜索II
题目链接：[传送门](https://leetcode-cn.com/problems/word-search-ii/)

## 题目描述：
给定一个 $m \times n$ 二维字符网格`board`和一个单词（字符串）列表`words`，找出所有同时在二维网格和字典中出现的单词。

单词必须按照字母顺序，通过**相邻的单元格**内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。

**提示**：

- `m == board.length`
- `n == board[i].length`
- $ 1 \leq m, n \leq 12$
- `board[i][j]`是一个小写英文字母
- $ 1 \leq $ `words.length` $ \leq 3 \times 10^4$
- $1 \leq$ `words[i].length` $ \leq 10$
- `words[i]`由小写英文字母组成
- `words`中的所有字符串互不相同

**示例**：

![](../_media/212-search.jpg)

- 输入：`board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]`
- 输出：`["eat","oath"]`

## 解决方案：
- 时间复杂度：$O(m \times n \times 4^l)$
- 空间复杂度：$O(k \times l)$
- 思路：前缀树+dfs+剪枝。将所有单词放到一个字典树中，若匹配到word中的单词，则将该单词从字典树中去除，不然会重复遍历，增加复杂度（剪枝技巧）。

## AC代码：
```go
type TrieNode struct {
	word string
	next [26]*TrieNode
}
func (t *TrieNode) Insert(word string) {
	cur := t
	for _, ch := range word {
		if cur.next[ch-'a'] == nil {
			cur.next[ch-'a'] = &TrieNode{}
		}
		cur = cur.next[ch-'a']
	}
	cur.word = word // 标记当前节点即为一个子串，这样做能省去很多复杂操作
}
var dirs = []struct{ x, y int }{{0, 1}, {0, -1}, {1, 0}, {-1, 0}}
func findWords(board [][]byte, words []string) []string {
	m := len(board)
	n := len(board[0])
	root := &TrieNode{}
	for _, word := range words {
		root.Insert(word)
	}
	res := make([]string, 0)
	for i, row := range board {
		for j := range row {
			ch := board[i][j]
			board[i][j] = '#'
			dfs(root.next[ch-'a'], i, j, m, n, board, &res)
			board[i][j] = ch
		}
	}
	return res
}
func dfs(curNode *TrieNode, x, y, m, n int, board [][]byte, res *[]string) { //注意用指针才能修改
	if curNode == nil {
		return
	}
	if curNode.word != "" { //不为空，说明当前路径匹配到 word 中的子串了，添加到结果列表里
		*res = append(*res, curNode.word)
		curNode.word = "" //剪枝，避免重复匹配
	}
	for _, dir := range dirs {
		dx, dy := dir.x+x, dir.y+y
		if check(dx, dy, m, n) && board[dx][dy] != '#' {
			ch := board[dx][dy]
			board[dx][dy] = '#'
			dfs(curNode.next[ch-'a'], dx, dy, m, n, board, res)
			board[dx][dy] = ch 
		}
	}
}
func check(x, y, m, n int) bool {
	return x >= 0 && y >= 0 && x < m && y < n
}
```