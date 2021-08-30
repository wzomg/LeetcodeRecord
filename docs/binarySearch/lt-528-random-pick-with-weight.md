# 528.按权重随机选择
题目链接：[传送门](https://leetcode-cn.com/problems/random-pick-with-weight/)

## 题目描述：
给定一个正整数数组w，其中w[i]代表下标i的权重（下标从0开始），请写一个函数pickIndex，它可以随机地获取下标i，选取下标i的概率与w[i]成正比。

例如，对于w=[1, 3]，挑选下标 0 的概率为`1 / (1 + 3) = 0.25`（即，25%），而选取下标 1 的概率为`3 / (1 + 3) = 0.75`（即，75%）。

也就是说，选取下标 i 的概率为`w[i] / sum(w) `。

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(1)$
- 思路：将数组平铺，比如数组位[1,3]，则平铺后变成[0, 1, 1, 1]，为了提高查找效率，则使用前缀和来代替操作，即为[1,4]，1->0，4->1。在[0,4)随机生成一个整数x，若x为0，则对应1的下标；若x为1，2，3，则对应的下标为1，所以应该二分查找比x大的第一个下标，也就是找元素为x+1的下标。

## AC代码：
- Java

```java
//手写二分搜索
class Solution {
    private Random random;
    private int[] preSum;
    private int len;
    public Solution(int[] w) {
        random = new Random();
        if (w == null || (len = w.length) == 0) return;
        preSum = new int[len];
        for (int i = 0; i < len; i++) {
            preSum[i] = (i > 0 ? preSum[i - 1] : 0) + w[i];
        }
    }
    //找到第一个大于target的元素下标
    public int pickIndex() {
        //在 [0, sum(w)) 中随机生成一个整数
        int target = random.nextInt(preSum[len - 1]);
        int lt = 0, rt = len - 1, mid, res = len;
        while (lt <= rt) {
            mid = ((rt - lt) >> 1) + lt;
            if (preSum[mid] > target) { //尽量靠左
                res = mid;
                rt = mid - 1;
            } else lt = mid + 1;
        }
        return res;
    }
}
```

- Go

```go
type Solution struct {
	preSum []int
}
func Constructor(w []int) Solution {
	for i := 1; i < len(w); i++ {
		w[i] += w[i-1]
	}
	return Solution{w}
}
func (st *Solution) PickIndex() int {
	//在 [0, sum(w)) 中随机生成一个整数
	val := rand.Intn(st.preSum[len(st.preSum)-1]) + 1
	return sort.SearchInts(st.preSum, val) //这个函数若找不到val，则返回待插入的下标
}
```