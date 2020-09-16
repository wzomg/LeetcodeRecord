# 11.盛最多水的容器
题目链接：[传送门](https://leetcode-cn.com/problems/container-with-most-water/)

## 题目描述：
给你`n`个非负整数 $a_1, a_2, \dots, a_n$，每个数代表坐标中的一个点 $(i, a_i)$ 。在坐标内画 `n`条垂直线，垂直线`i`的两个端点分别为 $(i, a_i)$ 和 `(i, 0)`。找出其中的两条线，使得它们与`x`轴共同构成的容器可以容纳最多的水。

**说明**：你不能倾斜容器，且`n`的值至少为`2`。

![](../_media/question_11.jpg)

<p>图中垂直线代表输入数组 <code>[1,8,6,2,5,4,8,3,7]</code>。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。</p>

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：双指针。若舍弃较长边，则构成的面积肯定变小；若丢弃较短边，则构成的面积可能增大，往可能增大的方向移动指针即可。

## AC代码：
```java
class Solution {
	public int maxArea(int[] arr) {
		int lt = 0, rt = arr.length - 1, res = 0;
		while (lt < rt) {
			res = Math.max(res, Math.min(arr[lt], arr[rt]) * (rt - lt));
			if (arr[lt] < arr[rt])
				lt++;
			else
				rt--;
		}
		return res;
	}
}
```