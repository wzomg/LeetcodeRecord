# 80.删除排序数组中的重复项II
题目链接：[传送门](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)

## 题目描述：
给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素最多出现**两次**，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地**修改输入数组**并在使用 $O(1)$ 额外空间的条件下完成。

**示例**:

- 给定`nums = [1,1,1,2,2,3]`，
- 函数应返回新长度`length = 5`, 并且原数组的前五个元素被修改为`1, 1, 2, 2, 3`。
- 你不需要考虑数组中超出新长度后面的元素。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：双指针。先初始化数组前2个相同的元素，然后遍历当前元素`nums[i]`和已归并为答案的列表尾元素进行对比，若两者不同则每次最多填2个`nums[i]`即可。

## AC代码：
```java
class Solution {
	public int removeDuplicates(int[] nums) {
		int len, pos = 0;
		if (nums == null || (len = nums.length) == 0)
			return 0;
		int i = 1;
		// 先填充前2位数
		if (i < len && nums[i - 1] == nums[i]) {
			pos++;
			i++;
		}
		for (; i < len; i++) {
			// 每次最多填充2个即可
			if (nums[pos] != nums[i]) {
				nums[++pos] = nums[i];
				if (i + 1 < len && nums[i] == nums[i + 1]) {
					nums[++pos] = nums[i + 1];
					i++;
				}
			}
		}
		// 返回 pos + 1 个数
		return pos + 1;
	}
}
```