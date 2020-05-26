# 4.寻找两个正序数组的中位数
题目链接：[传送门](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

## 题目描述：
给定两个大小为`m`和`n`的正序（从小到大）数组`nums1`和`nums2`。

请你找出这两个正序数组的中位数，并且要求算法的时间复杂度为 $O(log(m + n))$。

你可以假设`nums1`和`nums2`不会同时为空。

**示例**:

- `nums1 = [1, 3]`
- `nums2 = [2]`
- 则中位数是`2.0`

## 解决方案：
- 时间复杂度：$O(\log^{min(m,n)})$
- 空间复杂度：$O(1)$
- 思路：以下解析过程摘抄自力扣题解，个人觉得其讲得非常详细！带思考阅读和整理出来。

首先要理解中位数是怎么得来的？在统计中，中位数被用来：

> 将一个集合划分为两个长度相等的子集，其中一个子集中的元素总是大于另一个子集中的元素。  

首先，在任意位置`i`将 $\text{A}$ 划分成两个部分：
```
        left_A           |          right_A
A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
```
由于 $\text{A}$ 中有`m`个元素， 所以有`m+1`种划分的方法（$ i \in [0, m] $）。

> len(left_A) = i， len(right_A) = m − i。<br>
注意：当`i = 0`时，$\text{left\_A}$ 为空集， 而当`i = m`时, $\text{right\_A}$ 为空集。

采用同样的方式，在任意位置`j`将 $\text{B}$ 划分成两个部分：
```
        left_B           |          right_B
B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
```
将 $\text{left\_A}$ 和 $\text{left\_B}$ 放入一个集合，并将 $\text{right\_A}$ 和 $\text{right\_B}$ 放入另一个集合。 再把这两个新的集合分别命名为 $\text{left\_part}$ 和 $\text{right\_part}$：
```
        left_part        |         right_part
A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
```
当 $\text{A}$ 和 $\text{B}$ 的总长度是偶数时，如果可以确认：
- $\text{len}(\text{left\_part}) = \text{len}(\text{right\_part}) $
- $\max(\text{left\_part}) \leq \min(\text{right\_part}) $

那么，$\{\text{A}, \text{B}\}$ 中的所有元素已经被划分为相同长度的两个部分，且前一部分中的元素总是小于或等于后一部分中的元素。中位数就是前一部分的最大值和后一部分的最小值的平均值：`median=max(left_part)`。

第一个条件对于总长度是偶数和奇数的情况有所不同，但是可以将两种情况合并。第二个条件对于总长度是偶数和奇数的情况是一样的。

要确保这两个条件，只需要保证：
- $i+j=m−i+n−j$（当 $m+n$ 为偶数）或 $i + j = m - i + n - j + 1$（当 $m+n$ 为奇数）。等号左侧为前一部分的元素个数，等号右侧为后一部分的元素个数。将`i`和`j`全部移到等号左侧，我们就可以得到 $i+j = \frac{m + n + 1}{2}$。这里的分数结果只保留**整数部分**。
- $0 \leq i \leq m$，$0 \leq j \leq n$。如果我们规定 $\text{A}$ 的长度小于等于 $\text{B}$ 的长度，即 $m \leq n$。这样对于任意的 $i \in [0, m]$，都有 $j = \frac{m + n + 1}{2} - i \in [0, n]$，那么我们在`[0, m]`的范围内枚举`i`并得到`j`，就不需要额外的性质了。
    - 如果 $\text{A}$ 的长度较大，那么我们只要交换 $\text{A}$ 和 $\text{B}$ 即可。
    - 如果 $m > n$，那么得出的`j`有可能是**负数**。
- $B[j−1] \leq A[i]$ 以及 $\text{A}[i-1] \leq \text{B}[j] $，即前一部分的最大值小于等于后一部分的最小值。

为了简化分析，假设 $\text{A}[i-1], \text{B}[j-1], \text{A}[i], \text{B}[j]$ 总是存在。对于`i=0`、`i=m`、`j=0`、`j=n`这样的临界条件，我们只需要规定 $A[-1]=B[-1]=-\infty $，$A[m]=B[n]=\infty $ 即可。这也是比较直观的：当一个数组不出现在前一部分时，对应的值为负无穷，就不会对前一部分的最大值产生影响；当一个数组不出现在后一部分时，对应的值为正无穷，就不会对后一部分的最小值产生影响。

所以我们需要做的是：在`[0, m]`中找到`i`，使得：$ \text{B}[j-1] \leq \text{A}[i] $ 且 $\text{A}[i-1] \leq \text{B}[j] $，其中 $j = \frac{m + n + 1}{2} - i$。

我们证明它等价于：在`[0, m]`中找到最大的`i`，使得：$\text{A}[i-1] \leq \text{B}[j] $，其中 $j = \frac{m + n + 1}{2} - i$。这是因为：①当`i`从 $0 \sim m $ 递增时，$\text{A}[i-1]$ 递增，$\text{B}[j]$ 递减，所以一定存在一个最大的`i`满足 $A[i-1] \leq B[j]$；②如果`i`是最大的，那么说明`i+1`不满足。将`i+1`带入可以得到 $A[i] > B[j-1]$，也就是 $B[j - 1] < A[i]$，就和我们进行等价变换前`i`的性质一致了（甚至还要更强）。

因此我们可以对`i`在`[0, m]`的区间上进行二分搜索，找到最大的满足 $A[i-1] \leq B[j]$的`i`值，就得到了划分的方法。此时，划分前一部分元素中的最大值，以及划分后一部分元素中的最小值，才可能作为就是这两个数组的中位数。

## AC代码：
```java
class Solution {
	public double findMedianSortedArrays(int[] A, int[] B) {
		int m = A.length, n = B.length;
		if (m > n) { // 通过二分最小的数组长度避免取另外一个数组下标时出现越界
			int[] tmp1 = A;
			A = B;
			B = tmp1;
			int tmp2 = m;
			m = n;
			n = tmp2;
		}
		int lt = 0, rt = m, mid = (m + n + 1) >> 1; // 这样避免考虑2个数组长度之和的奇偶性，使得左半部分的长度最多比右半部分多一个元素
		while (lt <= rt) {
			int i = (lt + rt) >> 1; // 定位切割数组1的分界点
			int j = mid - i; // 定位切割数组2的分界点
			if (i < rt && B[j - 1] > A[i]) {
				// i < rt => j > 0 && B[j-1] <= A[i]
				lt = i + 1;
			} else if (i > lt && A[i - 1] > B[j]) {
				// i > lt => j < n && A[i-1] <= B[j]
				rt = i - 1;
			} else {
				// i is perfect
				int maxLeft = 0;
				if (i == 0) // 给出另外一方的值
					maxLeft = B[j - 1];
				else if (j == 0)
					maxLeft = A[i - 1];
				else // 取两个左半部分的最大值
					maxLeft = Math.max(A[i - 1], B[j - 1]);
				if ((m + n) % 2 == 1) // 若合并后是奇数长度，则直接返回左半部分的最大值
					return maxLeft;
				int minRight = 0;
				if (i == m) // 第一个数组右半部分为空，则给出另外一方的值
					minRight = B[j];
				else if (j == n) // 第二个数组右半部分为空，则给出另外一方的值
					minRight = A[i];
				else // 取2个右半部分的最小值
					minRight = Math.min(B[j], A[i]);
				return (maxLeft + minRight) / 2.0; // 返回中位数即可
			}
		}
		return 0.0;
	}
}
```