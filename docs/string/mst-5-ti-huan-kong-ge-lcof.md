# 面试题5.替换空格
题目链接：[传送门](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

## 题目描述：
请实现一个函数，把字符串`s`中的每个空格替换成`"%20"`。

**限制**：$ 0 \leq s $ 的长度 $ \leq 10000 $。

**示例**：

- 输入：`s = "We are happy."`
- 输出：`"We%20are%20happy."`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：调用API即可。

## AC代码：
```java
class Solution {
	public String replaceSpace(String s) {
		return s.replaceAll(" ", "%20");
	}
}
```