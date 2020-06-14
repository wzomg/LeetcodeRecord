# 1103.分糖果II
题目链接：[传送门](https://leetcode-cn.com/problems/distribute-candies-to-people/)

## 题目描述：
排排坐，分糖果。

我们买了一些糖果`candies`，打算把它们分给排好队的`n = num_people`个小朋友。

给第一个小朋友 $1$ 颗糖果，第二个小朋友 $2$ 颗，依此类推，直到给最后一个小朋友 $n$ 颗糖果。

然后，我们再回到队伍的起点，给第一个小朋友 $n + 1$ 颗糖果，第二个小朋友 $n + 2$ 颗，依此类推，直到给最后一个小朋友 $2 \times n$ 颗糖果。

重复上述过程（每次都比上一次多给出一颗糖果，当到达队伍终点后再次从队伍起点开始），直到我们分完所有的糖果。注意，就算我们手中的剩下糖果数不够（不比前一次发出的糖果多），这些糖果也会全部发给当前的小朋友。

返回一个长度为`num_people`、元素之和为`candies`的数组，以表示糖果的最终分发情况（即`ans[i]` 表示第`i`个小朋友分到的糖果数）。

**提示**：
- $1 \leq$ `candies` $ \leq 10^9$
- $1 \leq$ `num_people` $\leq 1000$

**示例1**：
- 输入：`candies = 7, num_people = 4`
- 输出：`[1,2,3,1]`
- 解释：
```
第一次，ans[0] += 1，数组变为 [1,0,0,0]。
第二次，ans[1] += 2，数组变为 [1,2,0,0]。
第三次，ans[2] += 3，数组变为 [1,2,3,0]。
第四次，ans[3] += 1（因为此时只剩下 1 颗糖果），最终数组变为 [1,2,3,1]。
```

**示例2**：
- 输入：`candies = 10, num_people = 3`
- 输出：`[5,2,3]`
- 解释：
```
第一次，ans[0] += 1，数组变为 [1,0,0]。
第二次，ans[1] += 2，数组变为 [1,2,0]。
第三次，ans[2] += 3，数组变为 [1,2,3]。
第四次，ans[0] += 4，最终数组变为 [5,2,3]。
```

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：等差数列求和，简单模拟一下即可！

## AC代码：
```java
class Solution {
	public int[] distributeCandies(int candies, int num_people) {
		int[] res = new int[num_people];
		if (candies == 0 || num_people == 0)
			return res;
		int n = 0, left = 0, shang, yu;
		while (((n + 1) * n >> 1) < candies)
			n++;
		if (((n + 1) * n >> 1) > candies) {
			n--;
			left = candies - ((n + 1) * n >> 1);
		}
		shang = n / num_people;
		yu = n % num_people;
		for (int i = 0; i < num_people; i++) {
			if (n < num_people) { // 直接填充
				if (candies > i + 1) {
					res[i] = i + 1;
					candies -= i + 1;
				} else {
					res[i] = candies;
					candies = 0;
				}
			} else {
				if (yu > 0) { // 等差数列求和
					res[i] = num_people * ((shang + 1) * shang >> 1) + (i + 1) * (shang + 1);
					yu--;
				} else {
					res[i] = num_people * ((shang - 1) * shang >> 1) + (i + 1) * shang;
					if (left > 0) { // 若有剩余糖果，则直接累加
						res[i] += left;
						left = 0;
					}
				}
			}
		}
		return res;
	}
}
```