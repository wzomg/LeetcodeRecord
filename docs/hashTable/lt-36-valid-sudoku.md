# 36.有效的数独
题目链接：[传送门](https://leetcode-cn.com/problems/valid-sudoku/)

## 题目描述：
请你判断一个 $ 9 \times 9$ 的数独是否有效。只需要 根据以下规则 ，验证已经填入的数字是否有效即可。
- 数字 $1-9$ 在每一行只能出现一次。
- 数字 $1-9$ 在每一列只能出现一次。
- 数字 $1-9$ 在每一个以粗实线分隔的 $3 \times 3$ 宫内只能出现一次。（请参考示例图）
- 数独部分空格内已填入了数字，空白格用`'.'`表示。

**注意**：

- 一个有效的数独（部分已被填充）不一定是可解的。
- 只需要根据以上规则，验证已经填入的数字是否有效即可。

**提示**：

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]`是一位数字或者`'.'`

## 解决方案：
- 时间复杂度：$O(1)$
- 空间复杂度：$O(1)$
- 思路：找规律+哈希。把 $ 9 \times 9$ 划分成9个9宫格，每个宫格的下标为：`box_idx = i / 3 * 3 + j / 3`，用行，列，宫格二维数组来标记是否出现 $ 1 \sim 9$，若已出现则直接返回false，否则标记对应下标出现过该数字。

## AC代码：
```go
func isValidSudoku(board [][]byte) bool {
	row, col, box := make([][]bool, 9), make([][]bool, 9), make([][]bool, 9)
	for i := 0; i < 9; i++ {
		row[i] = make([]bool, 10) //1 ~ 9
		col[i] = make([]bool, 10)
		box[i] = make([]bool, 10)
	}
	for i, r := range board {
		for j, c := range r {
			if c == '.' {
				continue
			}
			idx := i/3*3 + j/3
			if row[i][c-'0'] || col[j][c-'0'] || box[idx][c-'0'] {
				return false
			}
			row[i][c-'0'], col[j][c-'0'], box[idx][c-'0'] = true, true, true
		}
	}
	return true
}
```