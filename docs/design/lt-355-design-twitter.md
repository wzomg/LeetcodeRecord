# 355.设计推特
题目链接：[传送门](https://leetcode-cn.com/problems/design-twitter/)

## 题目描述：
设计一个简化版的推特`(Twitter)`，可以让用户实现发送推文，关注/取消关注其他用户，能够看见关注人（包括自己）的最近十条推文。你的设计需要支持以下的几个功能：

- `postTweet(userId, tweetId)`: 创建一条新的推文。
- `getNewsFeed(userId)`: 检索最近的十条推文。每个推文都必须是由此用户关注的人或者是用户自己发出的。推文必须按照时间顺序由最近的开始排序。
- `follow(followerId, followeeId)`: 关注一个用户。
- `unfollow(followerId, followeeId)`: 取消关注一个用户。


## 解决方案：
- 时间复杂度：$O(N × log^N)$
- 空间复杂度：$O(M)$
- 思路：哈希表+优先队列+单链表。对于第3和第4个功能，用一个`hashmap`来存储，`key`存放当前用户的`id`，`value`是一个`hashset`，用于存放当前用户关注的其他用户`id`，注意不能关注/取关自己！第1个功能的实现同样用一个`hashmap`来存储，`key`存放当前用户的`id`，`value`该怎么定义呢？因为第2个功能要取出最新的10条推文，且这10条推文中可能包括当前用户关注的人或者是该用户自己发出的。细细思考，可以用**单链表**来记录发送推文的时间顺序（**头插法**），然后借鉴合并k个有序链表的思想构建一个时间最大堆取出10个值即可！

## AC代码：
```java
class Twitter {
	// 记录推文发布的次序
	private int times;
	// 记录每个id关注的人
	private Map<Integer, Set<Integer>> follows;
	// 记录每个人发布的推文
	private Map<Integer, Tweets> posts;
	// 用来取最新的10条推文
	private Queue<Tweets> recentTenTweets;
	public Twitter() {
		times = 0;
		follows = new HashMap<Integer, Set<Integer>>();
		posts = new HashMap<Integer, Tweets>();
		// 维护一个最大堆，时间越大，说明最新
		recentTenTweets = new PriorityQueue<Tweets>((o1, o2) -> {
			return o2.postTime - o1.postTime;
		});
	}
	// 发布推文，使用头插法：O(1)
	public void postTweet(int userId, int tweetId) {
		// 时间点加1
		this.times++;
		Tweets newPost = new Tweets(tweetId, this.times), oldPost = posts.get(userId);
		newPost.next = oldPost;
		// 新值替换旧值
		posts.put(userId, newPost);
	}
	// 获取最新的10条推文：思路是合并k个有序链表：O(N × log^N)，N 为最大关注数
	public List<Integer> getNewsFeed(int userId) {
		// 获取当前用户关注的id列表
		Set<Integer> followsList = follows.get(userId);
		// 条件当前用户发布的推文单链表
		Tweets cur = posts.get(userId);
		// 链表不为空才添加到队列中
		if (cur != null)
			recentTenTweets.add(cur);
		// 循环添加关注者的推文单链表
		if (followsList != null) {
			for (Integer followee : followsList) {
				cur = posts.get(followee);
				// 若这个人没有发布推文，则不加入队列中
				if (cur != null)
					recentTenTweets.add(cur);
			}
		}
		// 存放最新10条推文的id
		List<Integer> res = new ArrayList<Integer>(10);
		// 统计取出10条推文
		int num = 0;
		Tweets head;
		while (!recentTenTweets.isEmpty()) {
			head = recentTenTweets.remove();
			if (num == 10) // 达到数量后循环清空队列
				continue;
			res.add(head.postTd);
			head = head.next;
			// 不为空才添加
			if (head != null)
				recentTenTweets.add(head);
			++num;
		}
		return res;
	}
	// 关注：O(1)
	public void follow(int followerId, int followeeId) {
		// 细节：不能关注自己
		if (followerId == followeeId)
			return;
		if (!follows.containsKey(followerId))
			follows.put(followerId, new HashSet<Integer>());
		follows.get(followerId).add(followeeId);
	}
	// 取关：O(1)
	public void unfollow(int followerId, int followeeId) {
		// 细节：不能取关自己
		if (followerId == followeeId)
			return;
		if (follows.containsKey(followerId))
			follows.get(followerId).remove(followeeId);
	}
}
// 构造一篇推文的存储结构
class Tweets {
	// 推文id，推文发布时间
	int postTd, postTime;
	// 上一条推文
	Tweets next;
	public Tweets(int postTd, int postTime) {
		this.postTd = postTd;
		this.postTime = postTime;
		this.next = null;
	}
}
```