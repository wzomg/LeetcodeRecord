# 面试题57-II.和为s的连续正数序列
题目链接：[传送门](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

## 题目描述：
输入一个正整数`target`，输出所有和为`target`的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

**限制**：$1 \leq $ `target` $ \leq 10^5$

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：双指针+等差数列求和。通过枚举区间两端点的值来求出该区间所有元素和是否满足target即可！

## AC代码：
```java
class Solution {
	public int[][] findContinuousSequence(int target) {
		List<int[]> res = new ArrayList<int[]>();
		int[] tmp;
		// 最终达到的上界为 target / 2
		for (int i = 1, j = 2, sum; i < j;) { // 注意，i必须小于j，才能保证子答案至少含2个元素
			sum = (i + j) * (j - i + 1) >> 1;
			if (sum == target) {
				tmp = new int[j - i + 1];
				for (int o = 0; o < j - i + 1; o++)
					tmp[o] = i + o;
				res.add(tmp);
				i++;
			} else if (sum < target)
				j++;
			else
				i++;
		}
		return res.toArray(new int[0][]);
	}
}
```