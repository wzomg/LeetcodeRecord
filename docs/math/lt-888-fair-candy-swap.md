# 888.公平的糖果交换
题目链接：[传送门](https://leetcode-cn.com/problems/fair-candy-swap/)

## 题目描述：
爱丽丝和鲍勃有不同大小的糖果棒：`A[i]`是爱丽丝拥有的第`i`块糖的大小，`B[j]`是鲍勃拥有的第`j`块糖的大小。

因为他们是朋友，所以他们想交换**一个糖果棒**，这样交换后，他们都有相同的糖果总量。（一个人拥有的糖果总量是他们拥有的糖果棒大小的总和。）

返回一个整数数组`ans`，其中`ans[0]`是爱丽丝必须交换的糖果棒的大小，`ans[1]`是`Bob`必须交换的糖果棒的大小。

如果有多个答案，你可以返回其中任何一个。保证答案存在。

**提示**：

- $ 1 \leq $ `A.length` $ \leq 10000 $
- $ 1 \leq $ `B.length` $ \leq 10000 $
- $ 1 \leq $ `A[i]` $ \leq 100000 $
- $ 1 \leq $ `B[i]` $ \leq 100000 $
- 保证爱丽丝与鲍勃的糖果总量不同。答案肯定存在。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：数学。注意题目中说了只是交换一个糖果棒，并且保证答案一定存在，那么假设A给B一个糖的大小为x，B给A一个糖的大小为y，则有等式：$ sum1 - x + y = sum2 - y + x $，化简得 $ y = x + \frac{sum2 - sum1}{2}$，接下来只需要遍历一遍数组即可。

## AC代码：
```java
class Solution {
	public int[] fairCandySwap(int[] A, int[] B) {
		int len1, len2, sum1 = 0, sum2 = 0, diff;
		if (A == null || (len1 = A.length) == 0 || B == null || (len2 = B.length) == 0)
			return new int[] { 0, 0 };
		boolean[] vis = new boolean[100001];
		for (int i = 0; i < len1; i++)
			sum1 += A[i];
		for (int i = 0; i < len2; i++) {
			sum2 += B[i];
			vis[B[i]] = true;
		}
		diff = (sum2 - sum1) >> 1;
		for (int i = 0, cur; i < len1; i++) {
			cur = A[i] + diff;
            // 注意加个判断，以防越界
			if (1 <= cur && cur <= 100000 && vis[cur])
				return new int[] { A[i], cur };
		}
		return new int[] { 0, 0 };
	}
}
```