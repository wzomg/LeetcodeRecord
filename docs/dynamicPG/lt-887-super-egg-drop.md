# 887.鸡蛋掉落
题目链接：[传送门](https://leetcode-cn.com/problems/super-egg-drop/)

## 题目描述：
你将获得`K`个鸡蛋，并可以使用一栋从`1`到`N`，共有`N`层楼的建筑。每个蛋的功能都是一样的，如果一个蛋碎了，你就不能再把它掉下去。

你知道存在楼层`F`，满足 $0 \leq F \leq N$ 任何从高于`F`的楼层落下的鸡蛋都会碎，从`F`楼层或比它低的楼层落下的鸡蛋都不会破。每次移动，你可以取一个鸡蛋（如果你有完整的鸡蛋）并把它从任一楼层`X`扔下（满足 $1 \leq X \leq N$ ）。

你的目标是确切地知道`F`的值是多少。无论`F`的初始值如何，你确定`F`的值的最小移动次数是多少？

## 解决方案：
- 时间复杂度：$O(K × N × log^N)$
- 空间复杂度：$O(K × N)$
- 思路：动态规划。定义 $dp(i,j)$ 表示`j`层楼用`i`个鸡蛋最坏情况下尝试的最小次数。我们尝试枚举`[1, n]`这`n`层楼中每一层楼作为起点开始尝试扔鸡蛋，假设在第`m`层楼开始尝试扔鸡蛋，那么第一次仍有两种情况：①碎：答案肯定在`[1, m - 1]`中，则一共需要尝试的次数为 $1+dp(k-1,m-1)$。②不碎：答案肯定在`[m + 1, n]`中，则一共需要尝试的次数为 $1+dp(k,n-m)$；所以在第`m`层楼开始扔鸡蛋最坏情况下需要尝试的次数是 $ A_m = 1 + max(dp(k-1, m-1), dp(k, n-m))$。因此，最终的答案就是在这`n`个作为起点开始尝试扔鸡蛋得到的最坏次数中取最小值 $ ans = min(A_1, A_2, \dots, A_n)$。注意：在枚举`[1, n]`过程中，可以发现函数 $dp(i,j)$ 是单调函数：`k`不变的情况下：$dp(k-1,m-1)$ 随着`m`的增加，其`dp`值也在增大，即这是一个单调递增函数；而 $dp(k,n-m)$ 随着`m`的减小，其`dp`值也在减小，即这个一个单调递减函数。因此，可以用二分查找这2个函数的交点。

## AC代码：
```java
class Solution {
	private int[][] dp;
	public int superEggDrop(int K, int N) {
		dp = new int[K + 1][N + 1];
		return dfs(K, N);
	}
	// K表示鸡蛋个数，N表示楼层数
	private int dfs(int K, int N) {
		if (K == 1) // 当只有一个鸡蛋时，最坏尝试次数为 N
			return N;
		if (N <= 1) // 当楼层数少于2时，最坏尝试次数肯定是0或1，直接返回 N 
			return N;
		if (dp[K][N] > 0) // 记忆化搜索
			return dp[K][N];
		int lt = 1, rt = N, res = N, mid, broken, nobroken;// 当前楼层最坏情况下，至多需要尝试n次
		// dfs(K - 1, mid - 1)和dfs(K, N - mid)是一个交叉线性函数，因此可用二分解法
		while (lt <= rt) {
			mid = (lt + rt) >> 1; // 尝试在第mid层楼下仍鸡蛋
			broken = dfs(K - 1, mid - 1); // 碎
			nobroken = dfs(K, N - mid); // 不碎
			if (broken > nobroken) {
				rt = mid - 1;
				res = Math.min(res, broken + 1); // 取最高点
			} else {
				lt = mid + 1;
				res = Math.min(res, nobroken + 1); // 取最高点
			}
		}
		return dp[K][N] = res;
	}
}
```