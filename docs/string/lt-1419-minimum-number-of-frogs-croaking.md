# 1419.数青蛙
题目链接：[传送门](https://leetcode-cn.com/problems/minimum-number-of-frogs-croaking/)

## 题目描述：
给你一个字符串`croakOfFrogs`，它表示不同青蛙发出的蛙鸣声（字符串`"croak"`）的组合。由于同一时间可以有多只青蛙呱呱作响，所以`croakOfFrogs`中会混合多个`“croak”`。请你返回模拟字符串中所有蛙鸣所需不同青蛙的最少数目。

注意：要想发出蛙鸣`"croak"`，青蛙必须 依序 输出`‘c’`,`’r’`,`’o’`,`’a’`,`’k’`这`5`个字母。如果没有输出全部五个字母，那么它就不会发出声音。

如果字符串`croakOfFrogs`不是由若干有效的`"croak"`字符混合而成，请返回`-1`。

**提示**：

- $1 \leq $ `croakOfFrogs.length` $ \leq 10^5$
- 字符串中的字符只有`'c'`,`'r'`,`'o'`,`'a'`或者`'k'`。

**示例1**：

- 输入：`croakOfFrogs = "croakcroak"`
- 输出：1 
- 解释：一只青蛙 “呱呱” 两次

**示例2**：

- 输入：`croakOfFrogs = "crcoakroak"`
- 输出：`2` 
- 解释：最少需要两只青蛙，“呱呱”声用黑体标注，第一只青蛙`"crcoakroak"`，第二只青蛙`"crcoakroak"`。

**示例3**：

- 输入：`croakOfFrogs = "croakcrook"`
- 输出：`-1`
- 解释：给出的字符串不是`"croak"`的有效组合。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：思维。题目中说了字符串的组成必须是由若干有效的`"croak"`混合，即该字符串中每个字符出现的顺序必须保持一致，并且同一时间可以有多只青蛙呱呱作响，所以我们要做的就是找出交错组成`"croak"`的最大数目。我们用`c`，`r`，`o`，`a`四个变量来记录字符出现的次数，当遇到字符`k`时，就把`c`，`r`，`o`，`a`数量都减一；最后判断`c`，`r`，`o`，`a`是否都为`0`，若为`0`则说明`croakOfFrogs`的确是一堆蛙叫声，注意：时刻保证需要保证 $ c \geq r \geq o \geq a$。

## AC代码：
```java
class Solution {
	public int minNumberOfFrogs(String croakOfFrogs) {
		int len, c = 0, r = 0, o = 0, a = 0, maxFrogs = 0;
		// 若字符串长度小于5或者最后一个字符不为'k'，则直接返回-1
		if (croakOfFrogs == null || (len = croakOfFrogs.length()) < 5 || croakOfFrogs.charAt(len - 1) != 'k')
			return -1;
		for (int i = 0; i < len; i++) {
			switch (croakOfFrogs.charAt(i)) {
			case 'c':
				c++;
				break;
			case 'r':
				r++;
				if (r > c)
					return -1;
				break;
			case 'o':
				o++;
				if (o > r)
					return -1;
				break;
			case 'a':
				a++;
				if (a > o)
					return -1;
				break;
			case 'k':
				// 青蛙的数量取决于交错字符组成croak的c字符个数
                // 若相邻子串都是croak，即croakcroak，则只需要一只青蛙
				maxFrogs = Math.max(maxFrogs, c);
				c--;
				r--;
				o--;
				a--;
				break;
			}
		}
		// 若不全为0，则直接返回-1
		if (c + r + o + a > 0)
			return -1;
		return maxFrogs;
	}
}
```