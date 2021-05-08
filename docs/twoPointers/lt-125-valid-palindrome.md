# 125.验证回文串
题目链接：[传送门](https://leetcode-cn.com/problems/valid-palindrome/)

## 题目描述：
给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

**说明**：本题中，我们将空字符串定义为有效的回文串。

**示例**：

- 输入: `"A man, a plan, a canal: Panama"`
- 输出: `true`

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：只考虑字母和数字字符，双指针简单判断即可。

## AC代码：
```java
class Solution {
    public boolean isPalindrome(String s) {
        int len;
        if (s == null || (len = s.length()) == 0) return true;
        char a, b;
        for (int i = 0, j = len - 1; i < j; i++, j--) {
            while (i < j && !check(s.charAt(i))) i++;
            while (i < j && !check(s.charAt(j))) j--;
            a = s.charAt(i);
            b = s.charAt(j);
            if (Character.isLetter(a)) a = Character.toLowerCase(a);
            if (Character.isLetter(b)) b = Character.toLowerCase(b);
            if (a != b) return false;
        }
        return true;
    }
    private boolean check(char ch) {
        return Character.isLetterOrDigit(ch);
    }
}
```