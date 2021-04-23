# 12.整数转罗马数字
题目链接：[传送门](https://leetcode-cn.com/problems/integer-to-roman/)

## 题目描述：
罗马数字包含以下七种字符：`I`，`V`，`X`，`L`，`C`，`D`和`M`。

``` 
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字`2`写做`II`，即为两个并列的`1`。`12`写做`XII`，即为`X + II`。`27`写做`XXVII`，即为`XX + V + II`。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如`4`不写做`IIII`，而是`IV`。数字`1`在数字`5`的左边，所表示的数等于大数`5`减小数`1`得到的数值`4`。同样地，数字`9`表示为`IX`。这个特殊的规则只适用于以下六种情况：

- `I`可以放在`V(5)`和`X(10)`的左边，来表示 4 和 9。
- `X`可以放在`L(50)`和`C(100)`的左边，来表示 40 和 90。 
- `C`可以放在`D(500)`和`M(1000)`的左边，来表示 400 和 900。

给定一个整数，将其转为罗马数字。输入确保在 1 到 3999 的范围内。

**提示**：$1 \le num \le 3999$

**示例1**:
- 输入: 3
- 输出: `"III"`

**示例2**:
- 输入: 1994
- 输出: `"MCMXCIV"`
- 解释: M = 1000, CM = 900, XC = 90, IV = 4.


## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：先保存所有可能出现的映射情况，再根据出现的数字从大到小依次贪心拼接其对应的字符串即可。

## AC代码：
```java
class Solution {
	public String intToRoman(int num) {
		Map<Integer, String> map = new HashMap<Integer, String>();
		map.put(1, "I");
		map.put(4, "IV");
		map.put(5, "V");
		map.put(9, "IX");
		map.put(10, "X");
		map.put(40, "XL");
		map.put(50, "L");
		map.put(90, "XC");
		map.put(100, "C");
		map.put(400, "CD");
		map.put(500, "D");
		map.put(900, "CM");
		map.put(1000, "M");
		if (map.containsKey(num))
			return map.get(num);
		int[] arr = new int[] { 1, 4, 5, 9, 10, 40, 50, 90, 100, 400, 500, 900, 1000 };
		StringBuilder sb = new StringBuilder();
		for (int i = arr.length - 1, cnt; i >= 0; i--) { //从大到小依次贪心
			if (num >= arr[i]) {
				cnt = num / arr[i];
				num %= arr[i];
				while (cnt-- > 0)
					sb.append(map.get(arr[i]));
			}
		}
		return sb.toString();
	}
}
```