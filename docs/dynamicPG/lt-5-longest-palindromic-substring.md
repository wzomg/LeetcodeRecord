# 5.最长回文子串
题目链接：[传送门](https://leetcode-cn.com/problems/longest-palindromic-substring/)

## 题目描述：
给定一个字符串`s`，找到`s`中最长的回文子串。你可以假设`s`的最大长度为`1000`。

**示例**：
- 输入: `"babad"`
- 输出: `"bab"`
- 注意: `"aba"` 也是一个有效答案。

## 解决方案：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n^2)$
- 思路：定义`dp[i][j]`：表示子串`[i,j]`是否构成回文子串，易推得状态转移方程为：当`s[i] == s[j]` 时，`dp[i][j] = j - i < 3 || dp[i + 1][j - 1]`，同时记录回文子串的最大长度即可！

## AC代码：
```java
class Solution {
    public String longestPalindrome(String s) {
        int len;
        if(s == null || (len = s.length()) == 0) return "";
        int begin = 0, res = 0;
        boolean[][] dp = new boolean[len][len];
        for(int j = 0; j < len; j++) { // 区间右端点
            for(int i = 0; i <= j; i++) { // 区间左端点，注意取等号
                if(s.charAt(i) == s.charAt(j)) { 
                    // 当前后两处字符相等时，若区间长度小于3，显然为true
                    dp[i][j] = j - i < 3 || dp[i + 1][j - 1];
                    if(dp[i][j] && j - i + 1 > res) { // 记录答案
                        begin = i;
                        res = j - i + 1;
                    } 
                }
            }
        }
        return s.substring(begin, begin + res);
    }
}
```
