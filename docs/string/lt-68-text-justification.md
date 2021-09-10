# 68.文本左右对齐
题目链接：[传送门](https://leetcode-cn.com/problems/text-justification/)

## 题目描述：
给定一个单词数组和一个长度maxWidth，重新排版单词，使其成为每行恰好有maxWidth个字符，且左右两端对齐的文本。

你应该使用“贪心算法”来放置给定的单词；也就是说，尽可能多地往每行中放置单词。必要时可用空格`' '`填充，使得每行恰好有maxWidth个字符。

要求尽可能均匀分配单词间的空格数量。如果某一行单词间的空格不能均匀分配，则左侧放置的空格数要多于右侧的空格数。

文本的最后一行应为左对齐，且单词之间不插入额外的空格。

**说明**:
- 单词是指由非空格字符组成的字符序列。
- 每个单词的长度大于0，小于等于`maxWidth`。
- 输入单词数组`words`至少包含一个单词。

**示例**:
- 输入:

```
words = ["This", "is", "an", "example", "of", "text", "justification."]
maxWidth = 16
```

- 输出:

```
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：字符串模拟。对于每一行，先确定能放多少个单词，这样就能得到剩余空格数，为了尽可能均匀分配单词间的空格数量，多出来的空格要尽量均匀地分配到前几个单词间。总体来说，对于填充空格，有3种情况：
  - 当前行是最后一行：单词左对齐，且单词之间应只有一个空格，在行末填充剩余空格；
  - 当前行不是最后一行，且只有一个单词：该单词左对齐，在行末填充空格；
  - 当前行不是最后一行，且不只一个单词：设当前行单词数为`cntW`，空格数为`cntSpaces`，我们需要将空格均匀分配在单词之间，则单词之间应至少有avgSpaces= $\lfloor \frac{cntSpaces}{cntW - 1} \rfloor $ 个空格，多出来`extraSpces = cntSpaces mod (cntW - 1)`个空格，应均匀分配在前`extraSpaces`个单词之间。因此，前`extraSpaces`个单词之间填充`avgSpaces+1`个空格，其余单词之间填充`avgSpaces`个空格。

## AC代码：
```go
//将 m 个含1个空字符的字符串拼接成一个新的字符串
func genBlank(m int) string {
	return strings.Repeat(" ", m)
}
func fullJustify(words []string, maxWidth int) (res []string) {
	lenW, rt := len(words), 0
	for {
		lt := rt //当前行第一个单词的位置
		tot := 0 //当前行单词长度之和，注意每个单词间至少有一个空格（rt - lt + 1 个单词，刚好需要 rt - lt 个空格）
		for rt < lenW && tot+len(words[rt])+rt-lt <= maxWidth {
			tot += len(words[rt])
			rt++
		}
		//若当前行是最后一行，则所有单词左对齐，并且单词之间只有1个空格，剩下补空格
		if rt == lenW {
			str := strings.Join(words[lt:], " ")
			res = append(res, str+genBlank(maxWidth-len(str)))
			return //记得返回
		}
		cntW := rt - lt //不用加1，因为 rt 是开区间
		cntSpaces := maxWidth - tot
		//若只有1个单词，则该单词左对齐，剩余补空格
		if cntW == 1 {
			res = append(res, words[lt]+genBlank(cntSpaces))
			continue
		}
		//当前不止一个空格
		avgSpaces := cntSpaces / (cntW - 1)  //单词间至少填充的空格数
		extraSpces := cntSpaces % (cntW - 1) //剩余空格数
		//左闭右开区间，因为尽可能均匀分配空格，所以剩余空格刚好给不包含lt的右边extraSpces个单词，并且其间用avgSpaces个空格隔开
		s1 := strings.Join(words[lt:lt+extraSpces+1], genBlank(avgSpaces+1))
		s2 := strings.Join(words[lt+extraSpces+1:rt], genBlank(avgSpaces))
		res = append(res, s1+genBlank(avgSpaces)+s2)
	}
}
```