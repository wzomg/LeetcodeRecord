# 224.基本计算器
题目链接：[传送门](https://leetcode-cn.com/problems/basic-calculator/)

## 题目描述：
实现一个基本的计算器来计算一个简单的字符串表达式的值。

字符串表达式可以包含左括号`(`，右括号`)`，加号`+`，减号`-`，非负整数和空格` `。

**说明**：

- 你可以假设所给定的表达式都是有效的。
- 请**不要**使用内置的库函数 eval。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：将中缀表达式转化为后缀表达式，然后求解后缀表达式即可！这题我就直接按照计算器来做了，后面有空来优化一下！
- 转化步骤如下：

```
1、初始化两个栈：s1，s2，分别用来存储运算符和中间结果；
2、从左至右扫描中缀表达式；
3、遇到操作数时，压入s2；
4、遇到运算符时，将其与s1栈顶运算符进行比较：   
    4.1、若s1为空或者栈顶运算符为左括号“(”，则直接压入s1；
    4.2、否则，若其优先级比s1栈顶运算符的优先级高，则直接压入s1；
    4.3、否则，将s1栈顶运算符弹出并压入s2，转到步骤(4.1)继续比较；
5、遇到括号时：
    5.1、若是左括号“(”，则直接压入s1；
    5.2、若是右括号，则依次弹出s1栈顶的运算符，并压入s2，直至遇到左括号为止，然后将这一对括号丢弃；
6、重复步骤2至5，直到表达式扫描结束；
7、将栈s1中剩余的运算符依次弹出并压入s2；
8、依次弹出s2栈顶元素并输出，结果的逆序即为中缀表达式对应的后缀表达式。
```

> 个人的碎碎念：2020年04月08号早上简历被腾讯云部门捞了，晚上7点一面，放弃了网易云音乐的笔试，失去了一个宝贵的机会（众所周知，进腾讯比网易难多了）。。。面试手撕1个工厂模式和5道算法题，其中最后一道是写一个计算器，其余4道题都比较简单。当时剩下没多少时间了，虽然我之前没写过这种题，但是当时1分钟内脑袋灵光一闪，突然蹦出了一个正确的求解步骤（面完后百度了一下发现自己的思路和求解逆波兰表达式完全一致），就跟面试官讲了讲自己的思路，讲完后他说还有括号这种情况怎么处理，emmm，我说不知道。。。面完下楼洗个澡上楼到官网一查看面试流程就变灰了。。。😭😭😭好了，不多bb，该补补算法了。吃一堑，长一智！💪

## AC代码：
```java
class Solution {
    public int calculate(String s) {
        int len;
        if (s == null || (len = s.length()) == 0) return 0;
        Stack<Character> stack = new Stack<>();
        List<String> sufExpr = new ArrayList<>();
        char ch;
        int num = -1;
        for (int i = 0; i < len; i++) {
            ch = s.charAt(i);
            //跳过空格
            if (ch == ' ' ) continue;
            if (!isOp(ch)) { //若为数字字符，则计算数字
                if (num == -1) num = ch - '0';
                else num = num * 10 + (ch - '0' );
            } else {
                //将之前计算得到的数字加入后缀表达式中
                if (num != -1) {
                    sufExpr.add(String.valueOf(num));
                    num = -1;//重置为初始状态
                } else if (ch == '+' || ch == '-' ) {
                    //特判某些情况：+3-1、-2+1、1-(+1+1)、(-1) ==> 0+3-1、0-2+1、1-(0+1+1)、(0-1)
                    if (sufExpr.size() == 0) sufExpr.add(String.valueOf(0));
                    else {
                        //运算符栈顶元素必须为左括号，并且当前字符为 + 或 -
                        if (!stack.isEmpty() && stack.peek() == '(' )
                            //往后缀表达式中先添加0
                            sufExpr.add(String.valueOf(0));
                    }
                }
                //若运算符栈为空或者为左括号，则直接入栈
                if (stack.isEmpty() || ch == '(' ) stack.push(ch);
                else {
                    //先判断一下右括号，再判断优先级
                    if (ch == ')' ) {
                        //若是右括号，则栈1一直出栈直到第一次遇到左括号为止，将出栈的运算符都添加到后缀表达式中
                        while (stack.peek() != '(' ) sufExpr.add(String.valueOf(stack.pop()));
                        //记得将左括号出栈
                        stack.pop();
                    } else {
                        //若栈1非空，就让当前运算符和栈顶运算符作优先级比较，直到返回false
                        while (!stack.isEmpty() && calPriority(ch, stack.peek())) {
                            sufExpr.add(String.valueOf(stack.pop()));
                        }
                        //记得将当前运算符压入栈中
                        stack.push(ch);
                    }
                }
            }
        }
        //最后一步，添加数字和
        if (num != -1) sufExpr.add(String.valueOf(num));
        //栈中运算符的优先级为递增顺序
        while (!stack.isEmpty()) {
            sufExpr.add(String.valueOf(stack.pop()));
        }
        return evalRPN(sufExpr);
    }
    //判断2个运算符的优先级，true表示要运算符出栈
    private boolean calPriority(char ch1, char ch2) {
        //特判栈顶运算符，防止弹出左括号
        if (ch2 == '(' ) return false;
        //若当前字符的优先级小于等于栈顶运算符，则需弹出栈顶元素到后缀表达式中
        if (ch1 == '+' || ch1 == '-' ) return true;
        if (ch2 == '+' || ch2 == '-' ) return false;
        return true;
    }
    private boolean isOp(char ch) {
        return ch == '+' || ch == '-' || ch == '*' || ch == '/' || ch == '(' || ch == ')';
    }
    //计算后缀表达式
    public int evalRPN(List<String> tokens) {
        int len;
        if (tokens == null || (len = tokens.size()) == 0) return 0;
        Stack<Integer> stack = new Stack<>();
        for (int i = 0, a, b, c; i < len; i++) {
            if (checkOp(tokens.get(i))) {
                b = stack.pop();
                a = stack.pop();
                switch (tokens.get(i)) {
                    case "+":
                        c = a + b;
                        break;
                    case "-":
                        c = a - b;
                        break;
                    case "*":
                        c = a * b;
                        break;
                    case "/":
                        c = a / b;
                        break;
                    default:
                        c = 0;
                }
                stack.push(c);
            } else stack.push(Integer.parseInt(tokens.get(i)));
        }
        return stack.pop();
    }
    private boolean checkOp(String str) {
        return str.equals("+") || str.equals("-") || str.equals("*") || str.equals("/");
    }
}
```