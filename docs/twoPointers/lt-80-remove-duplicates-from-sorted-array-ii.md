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
- 思路：双指针。

## AC代码：
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int len;
        if (nums == null || (len = nums.length) == 0) return 0;
        int pos = 0;
        for (int i = 0; i < len; i++) {
            //第3个位置-2那个位置上的元素与当前元素不相等才进行添加，表示最多只有2个重复元素
            if (pos < 2 || nums[pos - 2] != nums[i]) nums[pos++] = nums[i];
        }
        return pos;
    }
}
```