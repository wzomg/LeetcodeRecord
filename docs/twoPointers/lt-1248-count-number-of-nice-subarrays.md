# 1248.统计「优美子数组」
题目链接：[传送门](https://leetcode-cn.com/problems/count-number-of-nice-subarrays/)

## 题目描述：
给你一个整数数组`nums`和一个整数`k`。

如果某个**连续**子数组中恰好有`k`个奇数数字，我们就认为这个子数组是「优美子数组」。

请返回这个数组中「优美子数组」的数目。

**提示**：

- $1 \leq$ `nums.length` $ \leq 50000$
- $1 \leq$ `nums[i]` $\leq 10^5$
- $1 \leq k \leq$ `nums.length`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：双指针思想+前缀和。定义`pre[i]`为`[0...i]`中奇数的个数，则`pre[i]`可以由`pre[i−1]`递推而来，即：`pre[i]=pre[i-1]+(nums[i]&1)`。假设当 $ j > 0 \land j \leq i$ 时，`[j...i]`这个子数组里的奇数个数恰好为`k`，则有`pre[i]-pre[j-1]=k`，将等式变换得`pre[j-1]=pre[i]-k`，即每遍历到下标为`i`时，就统计一下`pre[i]-k`出现的个数即可！注意：初始化`cnt[0]=1`。

## AC代码：
```java
class Solution {
	public int numberOfSubarrays(int[] nums, int k) {
		int len, res = 0;
		if (nums == null || (len = nums.length) == 0)
			return 0;
		int[] cnt = new int[len + 1]; // 最多 len 个奇数
		// 若子数组最左边包括下标0，则0的前一个下标就是-1，用 cnt[0] = 1 来初始化表示
		cnt[0] = 1;
		for (int i = 0, pre = 0; i < len; i++) {
			pre = ((nums[i] & 1) == 1 ? 1 : 0) + pre;
			res += pre - k >= 0 ? cnt[pre - k] : 0;
			cnt[pre]++;
		}
		return res;
	}
}
```