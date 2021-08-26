# 881.救生艇
题目链接：[传送门](https://leetcode-cn.com/problems/boats-to-save-people/)

## 题目描述：
第i个人的体重为`people[i]`，每艘船可以承载的最大重量为limit。

每艘船最多可同时载两人，但条件是这些人的重量之和最多为limit。

返回载到每一个人所需的最小船数。(保证每个人都能被船载)。

**提示**：
- $ 1 \leq $ `people.length` $ \leq 50000$
- $ 1 \leq$  `people[i]` $ \leq$  `limit` $ \leq 30000$ 

**示例**：
- 输入：`people = [1,2], limit = 3`
- 输出：1
- 解释：1 艘船载 (1, 2)

## 解决方案：
- 时间复杂度：$O(n \times log^n)$
- 空间复杂度：$O(1)$
- 思路：简单排序+贪心+双指针，详解看代码注释。

## AC代码：
```go
//贪心，尽可能选择一个较小的和一个较大的做在一个船里
func numRescueBoats(people []int, limit int) int {
  //排序
	sort.Ints(people)
	lt, rt, res := 0, len(people)-1, 0
	for lt <= rt {
		if people[lt]+people[rt] > limit { //单独给 rt 分配一艘船，因为每个人的重量不超过limit
			rt--
		} else { //两个人的重量不超过limit，直接分配，固定 lt，小于rt大于lt的人都可以选，但为了让一艘船承更多的重量，就选较大的rt
			lt++
			rt--
		}
		res++
	}
	return res
}
```