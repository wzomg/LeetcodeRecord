# 面试题40.最小的k个数
题目链接：[传送门](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

## 题目描述：
输入整数数组`arr`，找出其中最小的`k`个数。例如，输入`4、5、1、6、2、7、3、8`这8个数字，则最小的4个数字是`1、2、3、4`。

## 解决方案：
- 时间复杂度：$O(k \times log^k)$
- 空间复杂度：$O(k)$
- 思路：构建一个最大堆来维护前k小，若堆顶元素大于某个元素值，则将该堆顶元素换为该元素，并调整堆结构。

## AC代码：
```java
class Solution {
	public int[] getLeastNumbers(int[] arr, int k) {
		int len, idx = 0;
		if (arr == null || (len = arr.length) == 0 || k == 0)
			return new int[0];
		if (k > len)
			k = len;
		Queue<Integer> queue = new PriorityQueue<Integer>(new Comparator<Integer>() {
			@Override
			public int compare(Integer o1, Integer o2) {
				return o2 - o1; // 降序排
			}
		});
		for (int i = 0, siz; i < len; i++) {
			siz = queue.size();
			if (siz < k)
				queue.add(arr[i]);
			else {
				if (queue.peek() > arr[i]) {
					queue.poll();
					queue.add(arr[i]);
				}
			}
		}
		int[] res = new int[k];
		while (!queue.isEmpty())
			res[idx++] = queue.poll();
		return res;
	}
}
```