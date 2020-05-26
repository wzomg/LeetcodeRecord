# 974.和可被K整除的子数组
题目链接：[传送门](https://leetcode-cn.com/problems/subarray-sums-divisible-by-k/)

## 题目描述：
给定一个整数数组`A`，返回其中元素之和可被`K`整除的（连续、非空）子数组的数目。

**提示**：

- $ 1 \leq $ `A.length` $ \leq 30000$
- $-10000 \leq $ `A[i]` $ \leq 10000$
- $2 \leq K \leq 10000$

**示例**：

- 输入：`A = [4,5,0,-2,-3,1]`, `K = 5`
- 输出：`7`
- 解释：有`7`个子数组满足其元素之和可被 $K = 5$ 整除：`[4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：同余定理+排列组合。若两个整数 $a, b$ 满足 $(a-b)\%K == 0$，那么有 $a\%K == b\%K$。由此可知，若 $i, j$ 满足 $(pre[j] - pre[i-1])\%K == 0$，则有 $pre[i]\%K == pre[i-1]\%K$。因此，我们应该从头到尾扫一遍数组，同时统计正余数各自出现的个数，最后用组合公式求个数累加答案即可！

## AC代码：
```java
class Solution {
	public int subarraysDivByK(int[] A, int K) {
		int len, res = 0, sum = 0;
		if (A == null || (len = A.length) == 0)
			return 0;
		Map<Integer, Integer> map = new HashMap<>();
		map.put(0, 1);
		for (int i = 0; i < len; i++) {
			sum = (sum + A[i] % K + K) % K; // 前缀和取余再取正数
			map.put(sum, map.getOrDefault(sum, 0) + 1);
		}
		for (Map.Entry<Integer, Integer> kv : map.entrySet()) // 排列组合
			res += (kv.getValue() * (kv.getKey() - 1)) >> 1;
		return res;
	}
}
```