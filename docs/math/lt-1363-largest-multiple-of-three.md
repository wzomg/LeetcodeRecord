# 1363.形成三的最大倍数
题目链接：[传送门](https://leetcode-cn.com/problems/largest-multiple-of-three/)

## 题目描述：
给你一个整数数组`digits`，你可以通过按任意顺序连接其中某些数字来形成`3`的倍数，请你返回所能得到的最大的`3`的倍数。

由于答案可能不在整数数据类型范围内，请以字符串形式返回答案。

如果无法得到答案，请返回一个空字符串。

**提示**：

- $1 \leq$ `digits.length` $ \leq 10^4$
- $0 \leq $ `digits[i]` $ \leq 9$
- 返回的结果不应包含不必要的前导零。

**示例1**：

- 输入：`digits = [8,1,9]`
- 输出：`"981"`

**示例2**：

- 输入：`digits = [8,6,7,1,0]`
- 输出：`"8760"`



## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：数学+贪心。为了使得结果最大（长度最长），先将数组求和再取余3，若余数为1，则先尝试删除1个1，否则删除2个2；若余数为2，则先尝试删除1个2，否则删除2个1；若余数为0，则直接取。最后，倒序倒出桶中的元素。注意：若首位为0，则直接返回"0"。

## AC代码：
```java
class Solution {
	public String largestMultipleOfThree(int[] digits) {
		int len, sum = 0;
		if (digits == null || (len = digits.length) == 0)
			return "";
		int[] cnt = new int[10];
		for (int i = 0; i < len; i++) {
			sum += digits[i];
			// 统计每个元素出现的次数
			cnt[digits[i]]++;
		}
		if (sum % 3 == 1) {
			if (!del(cnt, 1)) {
				del(cnt, 2);
				del(cnt, 2);
			}
		} else if (sum % 3 == 2) {
			if (!del(cnt, 2)) {
				del(cnt, 1);
				del(cnt, 1);
			}
		}
		StringBuilder sb = new StringBuilder();
		// 从大到小添加到字符串末尾
		for (int i = 9; i >= 0; i--) {
			while (cnt[i]-- > 0)
				sb.append(i);
		}
		// 判断一下高位是否为0
		if (sb.length() > 0 && sb.charAt(0) == '0')
			return "0";
		return sb.toString();
	}
	private boolean del(int[] cnt, int num) {
		// 从小到大删除，步长为3
		for (int i = num; i <= 9; i += 3) {
			if (cnt[i] > 0) {
				cnt[i]--;
				return true;
			}
		}
		return false;
	}
}
```