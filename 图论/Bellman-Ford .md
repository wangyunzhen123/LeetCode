#### [787. K 站中转内最便宜的航班](https://leetcode-cn.com/problems/cheapest-flights-within-k-stops/)





## Bellman-Ford 算法

每次松弛操作实际上是对相邻节点的访问，第n次松弛操作保证了所有深度为n的路径最短。由于图的最短路径最长不会经过超过|v| - 1条边，所以可知贝尔曼-福特算法所得为最短路径



这种算法的要点是这样的：
设X是结点 A 到 B 的最短路径上的一个结点。
若把路径 A→B 拆成两段路径 A→X 和 X→B，则每一段路径 A→X 和 X→B 也都分别是结点 A 到 X 和结点 X 到 B 的最短路径。



dp下标的含义：

dp【i】【e】表示第i次遍历到e的最短路径，而第i次遍历意味着从源点到目标点深度为i的路径一定最短



状态转移：

深度为i的最短路径是建立在深度为i - 1之上的



初始化：

一开始深度唯一的最短路为0，即源点到源点的距离



遍历：

遍历n -1次，每次对每条边进行遍历

```c++
 int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
	const int inf = INT_MAX / 2;
	vector<vector<int> > f(k + 2,vector<int>(n, inf));
	f[0][src] = 0;
	for (int i = 1; i <= k + 1; ++i) {
        //对每条边进行遍历，只需要知道边就行
		for (auto& t : flights) {
			int s = t[0], e = t[1], cost = t[2];
			f[i][e] = min(f[i][e], f[i - 1][s] + cost);
		}
	}
	int ans = inf;
	for (int i = 1; i <= k + 1; ++i) {
		ans = min(f[i][dst], ans);
	}
	return ans == inf ? -1 : ans;
}
```

