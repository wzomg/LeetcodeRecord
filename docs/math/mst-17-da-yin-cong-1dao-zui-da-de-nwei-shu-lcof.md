# 面试题17.打印从1到最大的n位数
题目链接：[传送门](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

## 题目描述：
输入数字`n`，按顺序打印出从`1`到最大的`n`位十进制数。比如输入`3`，则打印出`1`、`2`、`3` 一直到最大的`3`位数`999`。

**说明**：用返回一个整数列表来代替打印，`n`为正整数。

**示例**:

- 输入: `n = 1`
- 输出: `[1,2,3,4,5,6,7,8,9]`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：简单从1打印到最大的n位数。

## AC代码：
```java
class Solution {
	public int[] printNumbers(int n) {
		int maxNum = 1;
		while (n-- > 0) // 求10^n
			maxNum *= 10;
		maxNum--;// (10^n)-1
		int[] nums = new int[maxNum];
		for (int i = 0; i < maxNum; i++)
			nums[i] = i + 1;
		return nums;
	}
}
```