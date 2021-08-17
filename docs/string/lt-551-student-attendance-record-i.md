# 551.学生出勤记录I
题目链接：[传送门](https://leetcode-cn.com/problems/student-attendance-record-i/)

## 题目描述：
给你一个字符串s表示一个学生的出勤记录，其中的每个字符用来标记当天的出勤情况（缺勤、迟到、到场）。记录中只含下面三种字符：

- 'A'：Absent，缺勤
- 'L'：Late，迟到
- 'P'：Present，到场
如果学生能够 同时 满足下面两个条件，则可以获得出勤奖励：

按**总出勤**计，学生缺勤（'A'）严格 少于两天。

学生**不会**存在 连续 3 天或 3 天以上的迟到（'L'）记录。

如果学生可以获得出勤奖励，返回 true ；否则，返回 false 。

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$
- 思路：简单计数和判断即可。

## AC代码：
```go
func checkRecord(s string) bool {
	cntA, cntL, res := 0, 0, true
	for _, val := range s {
		if val == 'L' {
			cntL++
			if cntL > 2 {
				res = false
				break
			}
		} else {
			cntL = 0
			if val == 'A' {
				cntA++
				if cntA > 1 {
					res = false
					break
				}
			}
		}
	}
	return res
}
```