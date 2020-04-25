# 1418.点菜展示表
题目链接：[传送门](https://leetcode-cn.com/problems/display-table-of-food-orders-in-a-restaurant/)

## 题目描述：
给你一个数组`orders`，表示客户在餐厅中完成的订单，确切地说，`orders[i]=[`$customerName_i,tableNumber_i,foodItem_i$`]`，其中$customerName_i$ 是客户的姓名，$tableNumber_i$ 是客户所在餐桌的桌号，而 $foodItem_i$ 是客户点的餐品名称。

请你返回该餐厅的 点菜展示表 。在这张表中，表中第一行为标题，其第一列为餐桌桌号`Table`，后面每一列都是按**字母顺序排列**的餐品名称。接下来每一行中的项则表示每张餐桌订购的相应餐品数量，第一列应当填对应的桌号，后面依次填写下单的餐品数量。

**注意**：客户姓名不是点菜展示表的一部分。此外，表中的数据行应该按餐桌桌号**升序排列**。


**提示**：
- $ 1 \leq $ `orders.length` $ \leq 5 \times 10^4 $
- `orders[i].length == 3`
- $ 1 \leq customerName_i.length, foodItem_i.length \leq 20$
- $ customerName_i $ 和 $foodItem_i $ 由大小写英文字母及空格字符`' '`组成。
- $tableNumber_i$ 是`1`到`500`范围内的整数。

**示例**：

- 输入：

```
orders = [["David","3","Ceviche"],
["Corina","10","Beef Burrito"],
["David","3","Fried Chicken"],
["Carla","5","Water"],
["Carla","5","Ceviche"],
["Rous","3","Ceviche"]]
```

- 输出：

```
[["Table","Beef Burrito","Ceviche","Fried Chicken","Water"],
["3","0","2","1","0"],
["5","0","1","0","1"],
["10","1","0","0","0"]]
```

- 解释：

```
点菜展示表如下所示：
Table,Beef Burrito,Ceviche,Fried Chicken,Water
3    ,0           ,2      ,1            ,0
5    ,0           ,1      ,0            ,1
10   ,1           ,0      ,0            ,0
对于餐桌 3：David 点了 "Ceviche" 和 "Fried Chicken"，而 Rous 点了 "Ceviche"
而餐桌 5：Carla 点了 "Water" 和 "Ceviche"
餐桌 10：Corina 点了 "Beef Burrito" 
```

## 解决方案：
- 时间复杂度：$O(N)$
- 空间复杂度：$O(N)$
- 思路：简单模拟。用哈希表建立一一映射关系即可！

## AC代码：
```java
class Solution {
	public List<List<String>> displayTable(List<List<String>> orders) {
		int len = orders.size();
		// key：餐桌号，value：该餐桌上每个餐品对应的数量
		Map<Integer, Map<String, Integer>> map = new HashMap<Integer, Map<String, Integer>>();
		List<String> list;
		String foodItem;
		// 存放餐品的种类名
		Map<String, Boolean> foods = new HashMap<String, Boolean>();
		Map<String, Integer> cnt;
		for (int i = 0, tableId; i < len; i++) {
			list = orders.get(i);
			// 餐桌号
			tableId = Integer.parseInt(list.get(1));
			// 餐品
			foodItem = list.get(2);
			foods.put(foodItem, true);
			if (!map.containsKey(tableId))
				map.put(tableId, new HashMap<String, Integer>());
			cnt = map.get(tableId);
			cnt.put(foodItem, cnt.getOrDefault(foodItem, 0) + 1);
			// 记得更新餐桌号对应的所有餐品
			map.put(tableId, cnt);
		}
		int[] nums = new int[map.size()];
		int idx = 0;
		// 取出餐桌号并排序
		for (int key : map.keySet())
			nums[idx++] = key;
		Arrays.sort(nums);
		// 存放最终答案
		List<List<String>> res = new ArrayList<List<String>>();
		// 名称
		List<String> first = new ArrayList<String>(), tmp;
		String[] fds = new String[foods.size()];
		int pos = 0;
		first.add("Table");
		// 取出每种餐品的名称
		for (String item : foods.keySet())
			fds[pos++] = item;
		// 按字典序排列
		Arrays.sort(fds);
		for (int i = 0; i < pos; i++)
			first.add(fds[i]);
		res.add(first);
		for (int i = 0; i < idx; i++) {
			tmp = new ArrayList<String>();
			// 添加餐号
			tmp.add(nums[i] + "");
			// 取出餐品种类
			cnt = map.get(nums[i]);
			// 遍历该餐桌上取出每种餐品对应的数量
			for (int j = 1, num; j < first.size(); j++) {
				foodItem = first.get(j);
				// 若没有则默认取0
				num = cnt.getOrDefault(foodItem, 0);
				tmp.add(num + "");
			}
			res.add(tmp);
		}
		return res;
	}
}
```