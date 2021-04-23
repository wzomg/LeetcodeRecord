# 875.爱吃香蕉的珂珂
题目链接：[传送门](https://leetcode-cn.com/problems/koko-eating-bananas/)

## 题目描述：
珂珂喜欢吃香蕉。这里有`N`堆香蕉，第`i`堆中有`piles[i]`根香蕉。警卫已经离开了，将在`H`小时后回来。

珂珂可以决定她吃香蕉的速度`K`（单位：根/小时）。每个小时，她将会选择一堆香蕉，从中吃掉`K`根。如果这堆香蕉少于`K`根，她将吃掉这堆的所有香蕉，然后这一小时内不会再吃更多的香蕉。  

珂珂喜欢慢慢吃，但仍然想在警卫回来前吃掉所有的香蕉。

返回她可以在`H`小时内吃掉所有香蕉的最小速度`K`（`K`为整数）。

## 解决方案：
- 时间复杂度：$O(n \times log^n)$
- 空间复杂度：$O(n)$
- 思路：典型的二分法。根据题意可知，吃香蕉的最大速度为数组中的最大值，最小值为1，即在区间`[1,maxV]`进行二分判断，若满足吃香蕉的时间不大于H，则不断缩小右边界即可。

## AC代码：
```java
class Solution {
	public int minEatingSpeed(int[] piles, int h) {
		int len, lt = 1, rt, res = 0;
		if (piles == null || (len = piles.length) == 0)
			return 0;
		int maxV = 0, mid;
		for (int i = 0; i < len; i++) // 吃香蕉的最大速度为数组中的最大值，最小值为1
			maxV = Math.max(maxV, piles[i]);
		rt = maxV;
		while (lt <= rt) {
			mid = (lt + rt) >> 1;
			if (check(piles, len, mid, h)) {
				rt = mid - 1;
				res = mid;
			} else
				lt = mid + 1;
		}
		return res;
	}
	private boolean check(int[] piles, int len, int k, int h) {
		int c = 0;
		for (int i = 0; i < len; i++) {
			if (piles[i] < k)
				c++;
			else
				c += piles[i] / k + (piles[i] % k != 0 ? 1 : 0);
		}
		return h >= c;
	}
}
```