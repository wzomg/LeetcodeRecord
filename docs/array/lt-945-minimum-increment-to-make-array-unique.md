# 945.使数组唯一的最小增量
题目链接：[传送门](https://leetcode-cn.com/problems/minimum-increment-to-make-array-unique/)

## 题目描述：
给定整数数组`A`，每次`move`操作将会选择任意`A[i]`，并将其递增`1`。

返回使`A`中的每个值都是唯一的最少操作次数。

**提示**：

- 0 <= A.length <= 40000
- 0 <= A[i] < 40000

## 解决方案：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$
- 思路：找规律。先排序再暴力模拟可以发现，每次都是得找第一个比当前数大且未出现的数字，且需要操作的次数为那个未出现的数字减去当前重复出现的数字。将所有这些做减法合并一下可知都对未出现的数字进行相加，对超过出现次数为1的数字进行相减。结合一下题目条件数组长度最大值为4w，假设数组元素都为39999，若采用O(n)暴力，至多需要79999的长度。于是接下来的做法就简单了，先统计每个元素出现的次数，然后暴力枚举，若有重复的则累加一下后面需要拿几个未出现的数字来凑即可！

## AC代码1：计数法
```java
class Solution {
	public int minIncrementForUnique(int[] A) {
		int len, res = 0, num = 0;
		if (A == null || (len = A.length) <= 1)
			return 0;
		int[] cnt = new int[79999];
		for (int i = 0; i < len; i++)
			cnt[A[i]]++;
		// 最坏情况下，需要遍历 79999 次
		for (int i = 0; i < 79999; i++) {
			if (cnt[i] > 0) {
				num += cnt[i] - 1;
				res -= (cnt[i] - 1) * i;
				len--; // 实际个数减去1
			} else if (num > 0 && cnt[i] == 0) {
				res += i;
				num--;
				len--;
			}
			if (len == 0) // 优化
				break;
		}
		return res;
	}
}
```

## AC代码2：排序+贪心
```java
class Solution {
    public int minIncrementForUnique(int[] A) {
        int len;
        if(A == null || (len = A.length) == 0) return 0;
        Arrays.sort(A); // 先排序 
        int preNum = A[0], res = 0;
        for(int i = 1; i < len; i++) {
            if(preNum + 1 <= A[i]) preNum = A[i];
            else { // preNum + 1 > A[i]
                res += preNum + 1 - A[i];
                preNum++;
            }
        }   
        return res;
    }
}
```