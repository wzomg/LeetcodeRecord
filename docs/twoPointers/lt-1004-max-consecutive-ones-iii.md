# 1004.最大连续1的个数III
题目链接：[传送门](https://leetcode-cn.com/problems/max-consecutive-ones-iii/)

## 题目描述：
给定一个由若干`0`和`1`组成的数组`A`，我们最多可以将`K`个值从`0`变成`1`。

返回仅包含`1`的最长（连续）子数组的长度。

**提示**：

- $1 \leq $ `A.length` $ \leq 20000$
- $ 0 \leq K \leq $ `A.length`
- `A[i]`为`0`或`1` 

**示例**：

- 输入：`A = [1,1,1,0,0,0,1,1,1,1,0]`, `K = 2`
- 输出：`6`
- 解释：`[1,1,1,0,0,1,1,1,1,1,1]`，粗体数字从`0`翻转到`1`，最长的子数组长度为`6`。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：双指针。取滑动窗口中`0`的个数不超过`k`时的最大长度即可。

## AC代码：
```java
class Solution {
	public int longestOnes(int[] A, int K) {
		int len, lt = 0, rt = 0, res = 0, cnt0 = 0;
		if (A == null || (len = A.length) == 0)
			return 0;
		while (rt < len) {
			if (A[rt] == 0)
				cnt0++;
			while (lt < rt && cnt0 > K) {
				if (A[lt] == 0)
					cnt0--;
				lt++;
			}
			if (cnt0 <= K) // 限制窗口中 0 的个数不超过 k 才可取其区间长度！
				res = Math.max(res, rt - lt + 1);
			rt++;
		}
		return res;
	}
}
```