# 167. 两数之和 II - 输入有序数组
题目链接：[传送门](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

## 题目描述：
给定一个已按照**升序排列**的有序数组，找到两个数使得它们相加之和等于目标数。
函数应该返回这两个下标值`index1`和`index2`，其中`index1`必须小于`index2`。

**说明**:
- 返回的下标值（`index1`和`index2`）不是从零开始的。
- 你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：双指针。

## AC代码：
```java
class Solution {
	public int[] twoSum(int[] numbers, int target) {
		int lt = 0, rt = numbers.length - 1;
		while (lt < rt) {
			if (numbers[lt] + numbers[rt] > target)
				rt--;
			else if (numbers[lt] + numbers[rt] < target)
				lt++;
			else // 当第一次出现相等时，直接break
				break;
		}
		return new int[] { lt + 1, rt + 1 };
	}
}
```