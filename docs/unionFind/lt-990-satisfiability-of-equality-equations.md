# 990.等式方程的可满足性
题目链接：[传送门](https://leetcode-cn.com/problems/satisfiability-of-equality-equations/)

## 题目描述：
给定一个由表示变量之间关系的字符串方程组成的数组，每个字符串方程`equations[i]`的长度为`4`，并采用两种不同的形式之一：`"a==b"`或`"a!=b"`。在这里，$a$ 和 $b$ 是小写字母（不一定不同），表示单字母变量名。

只有当可以将整数分配给变量名，以便满足所有给定的方程时才返回`true`，否则返回`false`。 

**示例1**：
- 输入：`["a==b","b!=a"]`
- 输出：`false`
- 解释：如果我们指定，$a = 1$ 且 $b = 1$，那么可以满足第一个方程，但无法满足第二个方程。没有办法分配变量同时满足这两个方程。

**示例2**：
- 输入：`["a==b","b!=c","c==a"]`
- 输出：`false`

## 解决方案：
- 时间复杂度：$O(n × log^n)$，可以简化为 $O(n)$：省去排序，分遍历2次。
- 空间复杂度：$O(n)$
- 思路：简单并查集，即判断已经合并过的元素是否有冲突。注意：先合并相等的元素，再去判断不等的元素，否则会出现漏判。

## AC代码：
```java
class Solution {
	private int[] fa;
	public boolean equationsPossible(String[] equations) {
		int len;
		if (equations == null || (len = equations.length) == 0)
			return true;
		Arrays.sort(equations, new Comparator<String>() {
			@Override
			public int compare(String s1, String s2) { // '=' > '!'
				return s2.charAt(1) - s1.charAt(1);
			}
		});
		boolean flag = true;
		fa = new int[26];
		for (int i = 0; i < 26; i++)
			fa[i] = i;
		for (int i = 0, _1, _4, _11, _44; i < len; i++) {
			_1 = equations[i].charAt(0) - 'a';
			_4 = equations[i].charAt(3) - 'a';
			if (equations[i].charAt(1) == '!') { // !=
				if (_1 == _4) { // 判断冲突
					flag = false;
					break;
				} else {
					_11 = find_fa(_1);
					_44 = find_fa(_4);
					if (_11 == _44) { // 判断冲突
						flag = false;
						break;
					}
				}
			} else if (_1 != _4) 
				unite(_1, _4);
		}
		return flag;
	}
	private int find_fa(int x) {
		return fa[x] == x ? x : (fa[x] = find_fa(fa[x]));
	}
	private void unite(int x, int y) {
		x = find_fa(x);
		y = find_fa(y);
		if (x != y)
			fa[x] = y;
	}
}
```