# LCP12.小张刷题计划
题目链接：[传送门](https://leetcode-cn.com/problems/xiao-zhang-shua-ti-ji-hua/)

## 题目描述：
为了提高自己的代码能力，小张制定了`LeetCode`刷题计划，他选中了`LeetCode`题库中的`n`道题，编号从 `0`到`n-1`，并计划在`m`天内**按照题目编号顺序**刷完所有的题目（注意，小张不能用多天完成同一题）。

在小张刷题计划中，小张需要用`time[i]`的时间完成编号`i`的题目。此外，小张还可以使用场外求助功能，通过询问他的好朋友小杨题目的解法，可以省去该题的做题时间。为了防止“小张刷题计划”变成“小杨刷题计划”，小张**每天最多使用一次求助**。

我们定义`m`天中做题时间最多的一天耗时为`T`（小杨完成的题目不计入做题总时间）。请你帮小张求出最小的 `T`是多少。

**限制**：

- $ 1 \leq $ `time.length` $ \leq 10^5$
- $ 1 \leq $ `time[i]` $\leq 10000$
- $ 1 \leq m \leq 1000$

**示例**：

- 输入：`time = [1,2,3,3]`, `m = 2`
- 输出：`3`
- 解释：第一天小张完成前三题，其中第三题找小杨帮忙；第二天完成第四题，并且找小杨帮忙。这样做题时间最多的一天花费了`3`的时间，并且这个值是最小的。

## 解决方案：
- 时间复杂度：$O(n × log^{\sum_{i=1}^{n} time[i]})$
- 空间复杂度：$O(1)$
- 思路：典型的二分求最大值最小化。因为要求分割成子数组中各自和减去该子数组中最大值后的最大值最小，所以我们只需在`[min(nums),sum(nums)]`中二分查找`mid`，判断其是否可以将整个数组划分成不超过`m`的子数组个数，并且不断夹逼最小化`mid`即可。判断子数组和时需要减去该子数组中最大的元素值。

## AC代码：
```java
class Solution {
	public int minTime(int[] time, int m) {
		int len = time.length, lt = Integer.MAX_VALUE, rt = 0, mid, res = 0;
		if (m >= len)
			return 0;
		for (int i = 0; i < len; i++) {
			lt = Math.min(lt, time[i]);
			rt += time[i];
		}
		while (lt <= rt) {
			mid = (lt + rt) >> 1;
			if (check(time, len, mid, m)) { // 切割得多了，调大下界
				lt = mid + 1;
			} else { // 切割得可能少了，调小上界
				rt = mid - 1;
				res = mid; // 同时记录值
			}
		}
		return res;
	}
	private boolean check(int[] time, int len, int limit, int m) {
		int cnt = 1, sum = time[0], maxVal = time[0]; // 初始化为1，且当前子数组最大值初始化为第一个元素
		for (int i = 1; i < len; i++) { // 从第二个元素开始遍历
			sum += time[i];
			maxVal = Math.max(maxVal, time[i]);
			if (sum - maxVal > limit) { // 划分 第 cnt + 1 个子数组（新的子数组的第一个元素）
				cnt++;
				maxVal = sum = time[i];
			}
			if (cnt > m) // 当划分的子数组个数超过m时，直接返回true
				return true;
		}
		return false; // 找到一种可能的分割方案
	}
}
```