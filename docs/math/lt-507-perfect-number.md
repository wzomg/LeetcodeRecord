# 507.完美数
题目链接：[传送门](https://leetcode-cn.com/problems/perfect-number/)

## 题目描述：
对于一个**正整数**，如果它和除了它自身以外的所有正因子之和相等，我们称它为“完美数”。

给定一个**整数**`n`，如果他是完美数，返回`True`，否则返回`False`。

**提示**：

输入的数字`n`不会超过`100000000`。 (1e8)

**示例**：

- 输入: `28`
- 输出: `True`
- 解释: `28 = 1 + 2 + 4 + 7 + 14`

## 解决方案：
- 时间复杂度：$O(\sqrt{n})$
- 空间复杂度：$O(1)$
- 思路：数学，只需枚举 $\sqrt{n}$ 内的所有因子，累加其所有值得到`sum`，最后判断一下 $sum==2 \times num $ 即可。

## AC代码：
```java
class Solution {
	public boolean checkPerfectNumber(int num) {
		if (num <= 0) // 不是正整数
			return false;
		int sum = 0;
		for (int i = 1; i * i <= num; i++) {
			if (num % i == 0) {
				sum += i;
				if (i * i != num) // 注意避免累加同一个因子
					sum += num / i;
			}
		}
		return sum - num == num;
	}
}
```