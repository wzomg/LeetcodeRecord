# 460.LFU缓存
## 题目描述：
请你为最不经常使用（LFU）缓存算法设计并实现数据结构。它应该支持以下操作：`get`和`put`。
- `get(key)`：如果键存在于缓存中，则获取键的值（总是正数），否则返回 -1。
- `put(key, value)`：如果键不存在，请设置或插入值。当缓存达到其容量时，则应该在插入新项之前，使最不经常使用的项无效。在此问题中，当存在平局（即两个或更多个键具有相同使用频率）时，应该去除 最近 最少使用的键。

「项的使用次数」就是自插入该项以来对其调用`get`和`put`函数的次数之和。使用次数会在对应项被移除后置为0。

**进阶**：你是否可以在O(1)时间复杂度内执行两项操作？

## 解决方案：
- 时间复杂度：O(1)
- 空间复杂度：O(n)
- 思路：2个哈希表，1个用来映射key对应双向链表中的一个节点，另一个用来映射频数对应的一个双向链表，同时维护一个频数最小的变量，这样操作的时间复杂度就大大降低！

## AC代码：
```java
class LFUCache {
	private Map<Integer, LFUCacheNode> map;
	// 用哈希表记录出现频数相同的双向链表
	private Map<Integer, LFUCacheDoubleLinkList> freqMap;
	// 实际存储的个数：size，给定容量：capacity
	private int size, capacity, minF;
	public LFUCache(int capacity) {
		map = new HashMap<Integer, LFUCacheNode>();
		freqMap = new HashMap<Integer, LFUCacheDoubleLinkList>();
		// 初始化最小值为1
		this.minF = 1;
		// 默认开一个频率为 1 的链表
		freqMap.put(1, new LFUCacheDoubleLinkList());
		this.capacity = capacity;
		this.size = 0;
	}
	public int get(int key) {
		int res = -1;
		if (map.containsKey(key)) {
			LFUCacheNode cur = map.get(key);
			res = cur.value;
			// 待操作：将访问次数加1
			updateFreq(cur);
		}
		return res;
	}
	public void put(int key, int value) {
		// 先判断是否命中
		if (map.containsKey(key)) {
			// 命中则新值覆盖旧值
			LFUCacheNode cur = map.get(key);
			cur.value = value;
			// 待操作：将访问次数加1
			updateFreq(cur);
			return;
		}
		// 不命中就创建一个新的节点
		LFUCacheNode newNode = new LFUCacheNode(key, value);
		// 新增一个节点在哈希表中
		map.put(key, newNode);
		// 并且加入到出现频数为1的双向链表中
		LFUCacheDoubleLinkList dbLinkList = freqMap.get(1);
		// 添加到双向链表的头节点
		dbLinkList.addFirst(newNode);
		// 判断当前节点数量是否超过给定容量
		if (this.size < this.capacity)
			this.size++;
		else {
			// 取出当前最小节点
			LFUCacheDoubleLinkList curLinkList = freqMap.get(minF);
			LFUCacheNode lastCacheNode = curLinkList.removeLast();
			// 删除哈希表中某个key
			map.remove(lastCacheNode.key);
		}
		// 更新出现的最小频率
		this.minF = 1;
	}
	// 更新该节点出现的次数
	private void updateFreq(LFUCacheNode target) {
		// 获取待更新节点的双向链表
		LFUCacheDoubleLinkList dbLinkList = freqMap.get(target.freq);
		// 删除该双向链表中的特定节点
		dbLinkList.remove(target);
		// 新的访问次数
		int newF = target.freq + 1;
		// 【易错】若旧链表里已清空，并且最低频率与当前频率相同，则更新当前最低频率
		if (this.minF == target.freq && dbLinkList.isEmpty())
			this.minF = newF;
		if (!freqMap.containsKey(newF))
			freqMap.put(newF, new LFUCacheDoubleLinkList());
		LFUCacheDoubleLinkList curLinkList = freqMap.get(newF);
		// 插入首部
		target.freq = newF;
		curLinkList.addFirst(target);
	}
}
// 构造双向链表中的节点数据类型
class LFUCacheNode {
	// key - value，key出现的频数：freq
	int key, value, freq;
	// 前驱和后继指针
	LFUCacheNode prev, next;
	public LFUCacheNode(int key, int value) {
		this.key = key;
		this.value = value;
		this.freq = 1;
		this.prev = this.next = null;
	}
}
// 构造出现相同频数的双向链表
class LFUCacheDoubleLinkList {
	// 虚拟一个哨兵节点，有助于增删查改
	private LFUCacheNode dummyHead, dummyTail;
	public LFUCacheDoubleLinkList() {
		this.dummyHead = new LFUCacheNode(-1, -1);
		this.dummyTail = new LFUCacheNode(-2, -2);
		this.dummyHead.next = this.dummyTail;
		this.dummyTail.prev = this.dummyHead;
	}
	// 在双向链表头节点新增一个节点，从头节点往后，表示出现相同频率的节点时间的新旧顺序
	public void addFirst(LFUCacheNode target) {
		// dummyHead -> (target) -> leftNode
		// 先切断虚拟头节点和第一个节点的链接
		LFUCacheNode leftNode = dummyHead.next;
		// 然后将新插入的节点分别指向头和下一个节点
		target.prev = dummyHead;
		target.next = leftNode;
		// 链接双向链表
		dummyHead.next = target;
		leftNode.prev = target;
	}
	// 删除相同频数的双向链表的尾节点，表示该节点出现已经是最旧
	public LFUCacheNode removeLast() {
		return remove(dummyTail.prev);
	}
	// 删除后返回该节点
	public LFUCacheNode remove(LFUCacheNode target) {
		if (this.isEmpty())
			return null;
		// prevNode -> target -> nextNode
		LFUCacheNode prevNode = target.prev;
		LFUCacheNode nextNode = target.next;
		prevNode.next = nextNode;
		nextNode.prev = prevNode;
		// 同时将前驱和后继指针指向null
		target.prev = target.next = null;
		return target;
	}
	// 判断是否为空
	public boolean isEmpty() {
		return this.dummyHead.next == this.dummyTail;
	}
}
```