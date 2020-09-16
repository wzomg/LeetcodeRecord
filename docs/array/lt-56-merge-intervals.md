# 56.合并区间
题目链接：[传送门](https://leetcode-cn.com/problems/merge-intervals/)

## 题目描述：
给出一个区间的集合，请合并所有重叠的区间。

**示例**：
- 输入: `[[1,3],[2,6],[8,10],[15,18]]`
- 输出: `[[1,6],[8,10],[15,18]]`
- 解释: 区间`[1,3]`和`[2,6]`重叠, 将它们合并为`[1,6]`。

**提示**：`intervals[i][0] <= intervals[i][1]`

## 解决方案：
- 时间复杂度：$O(n × log^n)$
- 空间复杂度：$O(n)$
- 思路：先按区间左端点升序排，再`O(n)`贪心依次合并即可！

## AC代码：
```java
class Solution {
	public int[][] merge(int[][] intervals) {
		int len, p = 0, minLt, maxRt;
		if (intervals == null || (len = intervals.length) == 0)
			return new int[0][]; // 返回一个空的二维数组
		Arrays.sort(intervals, new Comparator<int[]>() { // 外部比较器
			@Override
			public int compare(int[] o1, int[] o2) { // 区间左端点从小到大排序
				return o1[0] - o2[0];
			}
		});
		List<int[]> res = new ArrayList<int[]>();
		boolean flag;
		while (p < len) {
			minLt = intervals[p][0];
			maxRt = intervals[p][1]; // 维护右端点的最大值
			flag = true;
			// 当前右端点最大值不小于下一个区间的左端点值
			while (p < len - 1 && maxRt >= intervals[p + 1][0]) {
				p++;
				flag = false;
				maxRt = Math.max(maxRt, intervals[p][1]);
			}
			if (flag) // 表明当前区间不用与其他区间一起合并
				res.add(intervals[p]);
			else // 否则重新生成一个数组并合并
				res.add(new int[] { minLt, maxRt });
			p++;
		}
        // java中的强制类型转换只是针对单个对象的，不能使用强换的方法转换泛型
		return res.toArray(new int[0][]); 
		// 使用方法：toArray(new Object[0])
	}
}
```