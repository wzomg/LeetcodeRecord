# 789.逃脱阻碍者
题目链接：[传送门](https://leetcode-cn.com/problems/escape-the-ghosts/)

## 题目描述：
你在进行一个简化版的吃豆人游戏。你从`[0, 0]`点开始出发，你的目的地是`target = [xtarget, ytarget]`。地图上有一些阻碍者，以数组ghosts给出，第i个阻碍者从`ghosts[i] = [xi, yi]`出发。所有输入均为**整数坐标**。

每一回合，你和阻碍者们可以同时向东，西，南，北四个方向移动，每次可以移动到距离原位置1个单位 的新位置。当然，也可以选择不动。所有动作 同时发生。

如果你可以在任何阻碍者抓住你**之前**到达目的地（阻碍者可以采取任意行动方式），则被视为逃脱成功。如果你和阻碍者同时到达了一个位置（包括目的地）都不算是逃脱成功。

只有在你有可能成功逃脱时，输出true；否则，输出false。

**提示**：
- $ 1 \leq$ `ghosts.length` $ \leq 100$
- `ghosts[i].length == 2`
- $-10^4 \leq x_i, y_i \leq 10^4$
- 同一位置可能有多个阻碍者。
- `target.length == 2`
- $-10^4 \leq x_{target}, y_{target} \leq 10^4$

**示例1**：
- 输入：`ghosts = [[1,0],[0,3]], target = [0,1]`
- 输出：true
- 解释：你可以直接一步到达目的地 (0,1) ，在 (1, 0) 或者 (0, 3) 位置的阻碍者都不可能抓住你。 

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：求曼哈顿距离，比较明显的结论：若阻碍者到达目的地的时间不大于我到达目的地，则不可能逃脱成功，相关证明方法可以看官方题解。

## AC代码：
```go
func escapeGhosts(ghosts [][]int, target []int) bool {
	mDis := calc([]int{0, 0}, target)
	for _, v := range ghosts {
		hDis := calc(v, target)
		if hDis <= mDis { 
			return false
		}
	}
	return true
}
func calc(p1, p2 []int) int {
	return abs(p1[0]-p2[0]) + abs(p1[1]-p2[1])
}
func abs(a int) int {
	if a < 0 {
		return -a
	}
	return a
}
```