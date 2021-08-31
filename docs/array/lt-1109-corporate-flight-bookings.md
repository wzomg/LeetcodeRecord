# 1109.航班预订统计
题目链接：[传送门](https://leetcode-cn.com/problems/corporate-flight-bookings/)

## 题目描述：
这里有n个航班，它们分别从1到n进行编号。

有一份航班预订表bookings，表中第i条预订记录bookings[i] = [$first_i, last_i, seats_i$]意味着在从$first_i$到 $last_i$ （包含 $first_i$ 和 $last_i$ ）的 每个航班 上预订了$seats_i$个座位。

请你返回一个长度为n的数组answer，其中`answer[i]`是航班i上预订的座位总数。

**提示**：
- $ 1 \leq n \leq 2 \times 10^4$
- $1 \leq$ `bookings.length` $ \leq 2 \times 10^4$
- `bookings[i].length == 3`
- $1 \leq first_i \leq last_i \leq n$
- $1 \leq seats_i \leq 10^4$

**示例**：
- 输入：`bookings = [[1,2,10],[2,3,20],[2,5,25]], n = 5`
- 输出：`[10,55,45,25,25]`
- 解释：

```
航班编号        1   2   3   4   5
预订记录 1 ：   10  10
预订记录 2 ：       20  20
预订记录 3 ：       25  25  25  25
总座位数：      10  55  45  25  25
因此，answer = [10,55,45,25,25]
```


## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：差分数组+前缀和，简单模拟一下就明白了。

## AC代码：
```go
func corpFlightBookings(bookings [][]int, n int) []int {
	res := make([]int, n)
	for _, v := range bookings {
		res[v[0]-1] += v[2]
		if v[1] < n { //小于n才处理右边界
			res[v[1]] -= v[2]
		}
	}
	for i := 1; i < n; i++ {
		res[i] += res[i-1]
	}
	return res
}
```