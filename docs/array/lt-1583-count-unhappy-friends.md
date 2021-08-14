# 1583.统计不开心的朋友
题目链接：[传送门](https://leetcode-cn.com/problems/count-unhappy-friends/)

## 题目描述：
给你一份`n`位朋友的亲近程度列表，其中`n`总是**偶数**。

对每位朋友`i`，`preferences[i]`包含一份**按亲近程度从高到低排列**的朋友列表。换句话说，排在列表前面的朋友与`i`的亲近程度比排在列表后面的朋友更高。每个列表中的朋友均以`0`到`n-1`之间的整数表示。

所有的朋友被分成几对，配对情况以列表`pairs`给出，其中`pairs[i] = [xi, yi]`表示`xi`与`yi`配对，且`yi`与`xi`配对。

但是，这样的配对情况可能会是其中部分朋友感到不开心。在`x`与`y`配对且`u`与`v`配对的情况下，如果同时满足下述两个条件，`x`就会不开心：

`x`与`u`的亲近程度胜过`x`与`y`，且`u`与`x`的亲近程度胜过`u`与`v`，返回**不开心的朋友的数目**。

**提示**：

- $2 \leq n \leq 500$
- n 是偶数
- `preferences.length == n`
- `preferences[i].length == n - 1`
- $ 0 \leq $ `preferences[i][j]` $ \leq n - 1$
- `preferences[i]`不包含`i`
- `preferences[i]`中的所有值都是独一无二的
- `pairs.length` == $ \frac{n}{2}$
- `pairs[i].length == 2`
- `xi != yi`
- $ 0 \leq x_i, y_i \leq n - 1$
- 每位朋友都**恰好**被包含在一对中

**示例**：

- 输入：`n = 4`, `preferences = [[1, 2, 3], [3, 2, 0], [3, 1, 0], [1, 2, 0]], pairs = [[0, 1], [2, 3]]`
- 输出：`2`
- 解释：

```
朋友 1 不开心，因为：
- 1 与 0 配对，但 1 与 3 的亲近程度比 1 与 0 高，且
- 3 与 1 的亲近程度比 3 与 2 高。
朋友 3 不开心，因为：
- 3 与 2 配对，但 3 与 1 的亲近程度比 3 与 2 高，且
- 1 与 3 的亲近程度比 1 与 0 高。
朋友 0 和 2 都是开心的。
```

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n^2)$
- 思路：简单模拟，详解看代码。

## AC代码：
```go
func unhappyFriends(n int, preferences [][]int, pairs [][]int) (res int) {
	pos := make([][]int, n)
	for i := 0; i < n; i++ {
		pos[i] = make([]int, n)
		for j := 0; j < n-1; j++ { 
			pos[i][preferences[i][j]] = j // 保存亲近程度的位置
		}
	}
	match := make([]int, n)
	for _, v := range pairs { // 两两匹配
		match[v[0]] = v[1]
		match[v[1]] = v[0]
	}
	for x, y := range match {
		for _, u := range preferences[x][:pos[x][y]] { // 遍历 x 与 y 亲近程度的前几个元素，看是否有u与x的亲近程度大于u与v
			v := match[u]
			if pos[u][x] < pos[u][v] {
				res++
				break
			}
		}
	}
	return
}
```