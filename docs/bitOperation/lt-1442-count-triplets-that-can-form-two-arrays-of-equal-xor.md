# 1442.形成两个异或相等数组的三元组数目
题目链接：[传送门](https://leetcode-cn.com/problems/count-triplets-that-can-form-two-arrays-of-equal-xor/)

## 题目描述：
给你一个整数数组`arr`。

现需要从数组中取三个下标`i`、`j`和`k`，其中`(0 <= i < j <= k < arr.length)`。

`a`和`b`定义如下：

`a = arr[i] ^ arr[i + 1] ^ ... ^ arr[j - 1]`

`b = arr[j] ^ arr[j + 1] ^ ... ^ arr[k]`

注意：`^`表示**按位异或**操作。

请返回能够令`a == b`成立的三元组`(i, j , k)`的数目。

**提示**：

- $ 1 \leq $ `arr.length` $ \leq 300$
- $1 \leq $ `arr[i]` $ \leq 10^8 $

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n)$
- 思路：前缀和思想。获取区间`[i,j]`的异或值：`val=xor[j]^xor[i-1]`，若其值为0，则肯定有`j - i - 1`个满足条件的三元组。

## AC代码：
```java
class Solution {
	public int countTriplets(int[] arr) {
		int len;
		if (arr == null || (len = arr.length) == 0)
			return 0;
		int[] res = new int[len + 1];
		for (int i = 1; i <= len; i++) // 前缀异或值
			res[i] = res[i - 1] ^ arr[i - 1];
		int ans = 0;
		for (int i = 0; i < len - 1; i++) { // 至少保留2个相邻的下标
			for (int j = i + 1; j <= len; j++) {
				if ((res[i] ^ res[j]) == 0)
					ans += j - i - 1;
			}
		}
		return ans;
	}
}
```
