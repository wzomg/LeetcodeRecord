# 93.复原IP地址
题目链接：[传送门](https://leetcode-cn.com/problems/restore-ip-addresses/)

## 题目描述：
给定一个只包含数字的字符串，复原它并返回所有可能的`IP`地址格式。

有效的`IP`地址 正好由四个整数（每个整数位于`0`到`255`之间组成，且不能含有前导`0`），整数之间用`'.'`分隔。

例如：`"0.1.2.201"`和`"192.168.1.1"`是**有效的IP地址**，但是`"0.011.255.245"`、`"192.168.1.312"`和`"192.168@1.1"`是**无效的IP地址**。

**提示**：

- $0 \leq $ `s.length` $ \leq 3000$；
- `s`仅由数字组成。

**示例**：

- 输入：`s = "101023"`
- 输出：`["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]`

## 解决方案：
- 时间复杂度：$O(3^{SEG\_COUNT} \times |s|)$，用`SEG_COUNT=4`表示`IP`地址的段数，乘以`|s|`是因为复原出了一种满足题目要求的`IP`地址需要的时间复杂度。
- 空间复杂度：$O(SEG\_COUNT)$，递归深度。
- 思路：深搜找出构成`IP`地址的4个数字，每个数字位数不超过3位即可。

## AC代码：
```java
class Solution {
	private List<String> res;
	public List<String> restoreIpAddresses(String s) {
		res = new ArrayList<>();
		int len;
		if (s == null || (len = s.length()) == 0)
			return res;
		dfs(s, 0, len, new ArrayList<>());
		return res;
	}
	private void dfs(String s, int cur, int len, List<String> segement) {
		if (cur == len) {
			if (segement.size() == 4) {
				StringBuilder sb = new StringBuilder();
				for (int i = 0; i < 3; i++) {
					sb.append(segement.get(i));
					sb.append(".");
				}
				sb.append(segement.get(3));
				// toString() 方法内部重新 new 了一个字符串
				res.add(sb.toString());
			}
			return;
		}
		// 最多是3份，才能继续遍历添加
		if (segement.size() >= 4)
			return;
		// 最多3位
		for (int i = cur; i < len && i < cur + 3; i++) {
			// 左闭右开区间
			String str = s.substring(cur, i + 1);
			// 不能是0开头，还有其他数字，这样就直接break
			if (str.length() > 1 && str.charAt(0) == '0')
				break;
			int num = Integer.parseInt(str);
			if (num >= 0 && num <= 255) {
				segement.add(str);
				// 下一个位置从 i + 1 开始
				dfs(s, i + 1, len, segement);
				segement.remove(segement.size() - 1);
			} else // 没必要继续遍历下去
				break;
		}
	}
}
```