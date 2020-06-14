# 1013.将数组分成和相等的三个部分
题目链接：[传送门](https://leetcode-cn.com/problems/partition-array-into-three-parts-with-equal-sum/)

## 题目描述：
给你一个整数数组`A`，只有可以将其划分为三个和相等的非空部分时才返回`true`，否则返回`false`。

形式上，如果可以找出索引 $i+1 < j$ 且满足 $A[0] + A[1] + \cdots + A[i] == A[i+1] + A[i+2] + \cdots + A[j-1] == A[j] + A[j-1] + \cdots + A[A.length - 1]$ 就可以将数组三等分。

**提示**：
- $3 \leq$ `A.length` $ \leq 50000$
- $-10^4 \leq$ `A[i]` $\leq 10^4$

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：因为要将数组分成和相等的三个部分，所以先统计一下数组所有元素之和，若其不能被3整除，则肯定不能分成3部分，否则贪心取2个`sum/3`的点，若第二个点不是最后一个位置，则数组肯定能分成3部分。

## AC代码：
```java
class Solution {
	public boolean canThreePartsEqualSum(int[] A) {
		int len, sum = 0, cnt = 0;
		if (A == null || (len = A.length) == 0)
			return false;
		for (int i = 0; i < len; i++)
			sum += A[i];
		if (sum % 3 != 0)
			return false;
		sum /= 3;
		for (int i = 0, num = 0; i < len; i++) {
			num += A[i];
			if (num == sum) {
				cnt++;
				num = 0;
			}
			if (cnt == 2 && i != len - 1)
				return true;
		}
		return false;
	}
}
```