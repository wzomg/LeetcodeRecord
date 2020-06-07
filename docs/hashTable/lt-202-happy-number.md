# 202.快乐数
题目链接：[传送门](https://leetcode-cn.com/problems/happy-number/)

## 题目描述：
编写一个算法来判断一个数 $n$ 是不是快乐数。

「快乐数」定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为1，也可能是**无限循环**但始终变不到1。如果**可以变为**1，那么这个数就是快乐数。

如果 $n$ 是快乐数就返回`True`；不是，则返回`False`。

**示例**：
- 输入：`19`
- 输出：`true`
- 解释：
$1^2 + 9^2 = 82$，
$8^2 + 2^2 = 68$，
$6^2 + 8^2 = 100$，
$1^2 + 0^2 + 0^2 = 1$。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：用一个哈希表来记录出现过的数字，若再次出现相同数，则表示无限循环；否则即可找到快乐数。

## AC代码：
```java
class Solution {
	public boolean isHappy(int n) {
		int x = 0;
		Map<Integer, Boolean> map = new HashMap<>();
		while (x != 1) {
			x = 0;
			while (n > 0) {
				x += (n % 10) * (n % 10);
				n /= 10;
			}
			if (map.containsKey(x))
				return false;
			map.put(x, true);
			n = x;
		}
		return true;
	}
}
```