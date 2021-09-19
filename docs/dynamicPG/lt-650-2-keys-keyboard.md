# 650.只有两个键的键盘
题目链接：[传送门](https://leetcode-cn.com/problems/2-keys-keyboard/)

## 题目描述：
最初记事本上只有一个字符`'A'`。你每次可以对这个记事本进行两种操作：

- Copy All（复制全部）：复制这个记事本中的所有字符（不允许仅复制部分字符）。
- Paste（粘贴）：粘贴**上一次**复制的字符。

给你一个数字`n`，你需要使用最少的操作次数，在记事本上输出**恰好**`n`个`'A'`。返回能够打印出n个`'A'`的最少操作次数。

**提示**：$ 1 \leq n \leq 1000$

**示例**：
- 输入：3
- 输出：3
- 解释：

```
最初, 只有一个字符 'A'。
第 1 步, 使用 Copy All 操作。
第 2 步, 使用 Paste 操作来获得 'AA'。
第 3 步, 使用 Paste 操作来获得 'AAA'。
```

## 解决方案：
- 时间复杂度：$O(n \times \sqrt{n})$
- 空间复杂度：$O(n)$
- 思路：数学+动态规划。定义：f[i]表示打印出i个`A`的最少操作数，因为不允许仅复制部分字符，假设当前有j个`A`，则需要先使用一次「复制全部」操作，再使用若干次「粘贴」操作得到 i 个`A`。显然，j 只能是 i 的因数，且「粘贴」操作的使用次数为  $\frac{i}{j} - 1 $，可得状态转移方程：f[i] = $ \min_{j | i} \{f[j] + \frac{i}{j} \}$，由于 i 肯定同时拥有因数 j 和 $\frac{i}{j}$，二者必有一个小于等于 $\sqrt{i}$。因此，只需在 $ [1, \sqrt{i}]$ 内枚举j，并做状态转移即可。 

## AC代码：
```go
func minSteps(n int) int {
	f := make([]int, n+1)
	for i := 2; i <= n; i++ {
		f[i] = math.MaxInt32
		for j := 1; j*j <= i; j++ { // sqrt(i) 内就能拿到 i 的所有因数
			if i%j == 0 {
				f[i] = min(f[i], f[j]+i/j)
				f[i] = min(f[i], f[i/j]+j)
			}
		}
	}
	return f[n]
}
func min(a, b int) int {
	if a < b {
		return a
	}
	return b
}
```