# 295.数据流的中位数
题目链接：[传送门](https://leetcode-cn.com/problems/find-median-from-data-stream/)

## 题目描述：
中位数是有序列表中间的数。如果列表长度是偶数，中位数则是中间两个数的平均值。

例如，[2,3,4] 的中位数是 3，[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

- void addNum(int num) - 从数据流中添加一个整数到数据结构中。
- double findMedian() - 返回目前所有元素的中位数。

**进阶**:
- 如果数据流中所有整数都在 0 到 100 范围内，你将如何优化你的算法？
- 如果数据流中 99% 的整数都在 0 到 100 范围内，你将如何优化你的算法？

**示例**：
```
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2
```

## 解决方案：
- 时间复杂度：$O(log^n)$
- 空间复杂度：$O(n)$
- 思路：维护两个堆，左边是大顶堆，右边是小顶堆，详解看代码注释。注意：go实现切片排序：要去实现sort.Interface中的3个接口（`Len() int`，`Less(i, j int) bool`，`Swap(i, j int)`），实现堆排序：要实现`container/heap`包中的2个接口（`Push(x interface{})`，`Pop() interface{}`）

## AC代码：
- Java
```java
class MedianFinder {
    private Queue<Integer> qLeft;
    private Queue<Integer> qRight;
    private int siz;
    public MedianFinder() {
        qLeft = new PriorityQueue<>((o1, o2) -> o2 - o1); //大顶堆
        qRight = new PriorityQueue<>();//小顶堆
        siz = 0;
    }
    public void addNum(int num) {
        if (siz % 2 == 0) {
            //偶数就先往右边堆插入，然后弹出右边堆顶元素到左边堆
            qRight.add(num);
            qLeft.add(qRight.remove());
        } else {
            //奇数就往做左边堆插入，然后弹出左边堆顶元素到右边堆
            qLeft.add(num);
            qRight.add(qLeft.remove());
        }
        siz++;
    }
    public double findMedian() {
        if (siz % 2 == 0)
            return ((qLeft.isEmpty() ? 0.0 : qLeft.peek()) + (qRight.isEmpty() ? 0.0 : qRight.peek())) * 0.5;
        return qLeft.isEmpty() ? 0.0 : qLeft.peek() * 1.0;
    }
}
```
- Go
```go
type MedianFinder struct {
	qMax hpMax //左边大顶堆
	qMin hpMin //右边小顶堆
	siz  int
}
func Constructor() MedianFinder {
	qMax := hpMax{make([]int, 0)}
	qMin := hpMin{make([]int, 0)}
	return MedianFinder{qMax, qMin, 0}
}
func (mf *MedianFinder) AddNum(num int) {
	if mf.siz%2 == 0 {
		heap.Push(&mf.qMin, num)
		heap.Push(&mf.qMax, heap.Pop(&mf.qMin).(int))
	} else {
		heap.Push(&mf.qMax, num)
		heap.Push(&mf.qMin, heap.Pop(&mf.qMax).(int))
	}
	mf.siz++
}
func (mf *MedianFinder) FindMedian() float64 {
	if mf.siz%2 == 0 {
		var v1, v2 int = 0, 0
		if mf.qMax.Len() > 0 {
			v1 = mf.qMax.arr[0]
		}
		if mf.qMin.Len() > 0 {
			v2 = mf.qMin.arr[0]
		}
		return float64(v1+v2) / 2
	}
	var v int = 0
	if mf.qMax.Len() > 0 {
		v = mf.qMax.arr[0]
	}
	return float64(v)
}
type hpMax struct {
	arr []int
}
//实现 sort.Interface 接口，即重写两个方法
func (h *hpMax) Push(v interface{}) {
	h.arr = append(h.arr, v.(int))
}
func (h *hpMax) Pop() interface{} {
	old := h.arr
	lastVal := old[len(old)-1]
	h.arr = old[:len(old)-1]
	return lastVal
}
// 这里决定大、小顶堆 现在是大顶堆
func (h hpMax) Less(i, j int) bool { return h.arr[i] > h.arr[j] }
func (h hpMax) Swap(i, j int) {
	h.arr[i], h.arr[j] = h.arr[j], h.arr[i]
}
func (h hpMax) Len() int { return len(h.arr) }
type hpMin struct {
	arr []int
}
func (h *hpMin) Push(v interface{}) {
	h.arr = append(h.arr, v.(int))
}
func (h *hpMin) Pop() interface{} {
	old := h.arr
	lastVal := old[len(old)-1]
	h.arr = old[:len(old)-1]
	return lastVal
}
//实现类似 sort.IntSlice 中的三个接口
// 这里决定大、小顶堆 现在是小顶堆
func (h hpMin) Less(i, j int) bool { return h.arr[i] < h.arr[j] }
func (h hpMin) Swap(i, j int) {
	h.arr[i], h.arr[j] = h.arr[j], h.arr[i]
}
func (h hpMin) Len() int { return len(h.arr) }
```