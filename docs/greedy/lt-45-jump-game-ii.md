# 45.跳跃游戏II
题目链接：[传送门](https://leetcode-cn.com/problems/jump-game-ii/)

## 题目描述：
给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

**说明**:

假设你总是可以到达数组的最后一个位置。


**示例**:

- 输入：`[2,3,1,1,4]`
- 输出：`2`
- 解释：跳到最后一个位置的最小跳跃数是 2。从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。


**提示**:

- $1 \leq$ `nums.length` $<= 1000$
- $0 \leq$ `nums[i]` $ \leq 10^5$


## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：贪心+模拟。站在当前位置，先检查是否跳一步就能直接到达`len - 1`；若不能，则尝试从`1~nums[cur]`中选择下次能跳得最远距离的位置。 

## AC代码：
```java
class Solution {
	public int jump(int[] nums) {
		int len, maxRt, maxIdx, cur = 0, res = 0;
		if (nums == null || (len = nums.length) == 0)
			return 0;
		while (cur < len - 1) { // cur 表示当前所处位置
			// 先检查一下当前位置跳一步是否可到达 len - 1，若是则跳一步即可
			if (cur + nums[cur] >= len - 1) {
				res++;
				break;
			}
			// 否则继续从当前位置向右在连续 nums[cur] 个元素中找到能跳得最远距离的位置
			maxIdx = cur + 1; // 至少跳1步到下一个位置
			maxRt = cur + 1 + nums[cur + 1];
			for (int i = 2; i <= nums[cur]; i++) { // 从2步开始
				if (maxRt >= len - 1) //剪枝
					break;
				if (cur + i + nums[cur + i] > maxRt) {
					maxRt = cur + i + nums[cur + i];
					maxIdx = cur + i;
				}
			}
			// 选择下次能跳得最远距离的下标
			cur = maxIdx;
			res++;
		}
		return res;
	}
}
```