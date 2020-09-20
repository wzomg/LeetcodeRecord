# 223.矩形面积
题目链接：[传送门](https://leetcode-cn.com/problems/rectangle-area/)

## 题目描述：
在**二维**平面上计算出两个**由直线构成的**矩形重叠后形成的总面积。

每个矩形由其左下顶点和右上顶点坐标表示，如图所示。

![](../_media/rectangle_area.png)

**示例**：

- 输入: `-3, 0, 3, 4, 0, -1, 9, 2`
- 输出: `45`
- 说明: 假设矩形面积不会超出`int`的范围。

## 解决方案：
- 时间复杂度：$O(1)$
- 空间复杂度：$O(1)$
- 思路：数学。总面积 = 单个面积1 - 重叠面积 + 单个面积2。
- 备注：酷家乐笔试题。

## AC代码：
```java
class Solution {
	public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
		int area1 = (C - A) * (D - B), area2 = (G - E) * (H - F);
		// (A, B)已在左下角 ，(C, D)已在右上角
		// (E, F)已在左下角，(G, H)已在右上角
		// 表示无重叠
		if (C <= E || G <= A || D <= F || H <= B)
			return area1 + area2;
		// 重叠的部分
		// 右上角取最小
		int topX = Math.min(C, G), topY = Math.min(D, H);
		// 左下角取最大
		int bottomX = Math.max(A, E), bottomY = Math.max(B, F);
		return area1 - (topX - bottomX) * (topY - bottomY) + area2;
	}
}
```