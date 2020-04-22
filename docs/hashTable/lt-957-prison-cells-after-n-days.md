# 957.N天后的牢房
题目链接：[传送门](https://leetcode-cn.com/problems/prison-cells-after-n-days/)

## 题目描述：
`8`间牢房排成一排，每间牢房不是有人住就是空着。

每天，无论牢房是被占用或空置，都会根据以下规则进行更改：

- 如果一间牢房的两个相邻的房间都被占用或都是空的，那么该牢房就会被占用。
- 否则，它就会被空置。
（请注意，由于监狱中的牢房排成一行，所以行中的第一个和最后一个房间无法有两个相邻的房间。）

我们用以下方式描述监狱的当前状态：如果第`i`间牢房被占用，则`cell[i]==1`，否则`cell[i]==0`。

根据监狱的初始状态，在`N`天后返回监狱的状况（和上述`N`种变化）。

**提示**：

- `cells.length == 8`
- `cells[i]`的值为`0`或`1` 
- $1 \leq N \leq 10^9$

## 解决方案：
- 时间复杂度：$O(256 × M)$
- 空间复杂度：$O(1)$
- 思路：因为只有`8`间房间，所以可将其表示成 $2^8=256$ 个状态，并且记录每个状态出现的时间点，若某个数第二次出现，则将`N`直接模掉循环节长度，`N`剩下的余数直接循环变换即可。

## AC代码：
```java
class Solution {
	public int[] prisonAfterNDays(int[] cells, int N) {
		int len = cells.length;
		int[] pre = new int[1 << len]; // 标记 2^len个数出现的时间点
		int[] arr = new int[len];
		int num = 0, times = 0;
		for (int i = 0; i < len; i++) {
			if (cells[i] == 1)
				num += (1 << i);
			pre[i] = -1;
		}
		boolean flag = false;
		while (N-- > 0) {
			num = 0;
			for (int i = 0; i < len; i++) {
				if (i == 0 || i == len - 1)
					arr[i] = 0;
				else if (cells[i - 1] == cells[i + 1]) {
					arr[i] = 1;
					num += (1 << i);
				} else
					arr[i] = 0;
			}
			for (int i = 0; i < len; i++)
				cells[i] = arr[i];
			if (flag)
				continue;
			times++;
			if (pre[num] > 0) {
				flag = true;
				N %= times - pre[num];
			}
			pre[num] = times;
		}
		return cells;
	}
}
```