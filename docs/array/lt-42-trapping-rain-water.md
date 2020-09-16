# 42.接雨水
题目链接：[传送门](https://leetcode-cn.com/problems/trapping-rain-water/)

## 题目描述：
给定`n`个非负整数表示每个宽度为`1`的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

![接雨水](../_media/rainwatertrap.png)

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：前缀和+简单模拟。每次找到比当前最高的高度时就统计一下答案，左右各扫一遍即可！

## AC代码：
```java
class Solution {
	public int trap(int[] height) {
		int len, ans = 0;
		if (height == null || (len = height.length) == 0)
			return 0;
		int[] sum = new int[len];
		int preIndex = 0, preMax = 0;
		// 从左往右
		for (int i = 0; i < len; i++) {
			sum[i] = (i > 0 ? sum[i - 1] : 0) + height[i];
            // 正向遍历取等号，反向遍历就不用，否则会重复计算
			if (height[i] >= preMax) { 
				ans += (i - 1 - preIndex) * preMax;
                // 注意：若 i == 0，则应取 sum[preIndex]，不能直接取0（相减会变成负数），否则取 sum[i - 1]
				ans -= (i - 1 >= 0 ? sum[i - 1] : sum[preIndex]) - sum[preIndex];
				preMax = height[i];
				preIndex = i;
			}
		}
		preIndex = len - 1;
		preMax = 0;
		// 从右往左
		for (int i = len - 1; i >= 0; i--) {
			sum[i] = (i + 1 < len ? sum[i + 1] : 0) + height[i];
            // 反向遍历就不用取等号，不然会重复计算值
			if (height[i] > preMax) {
				ans += (preIndex - 1 - i) * preMax;
				ans -= (i + 1 < len ? sum[i + 1] : sum[preIndex]) - sum[preIndex];
				preMax = height[i];
				preIndex = i;
			}
		}
		return ans;
	}
}
```