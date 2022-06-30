# 迪杰斯特拉算法：

问题定义

求解单元点的最短路径问题：给定**带权有向图G**和源点v，求v到G中其他顶点的最短路径

**限制条件：图G中不存在负权环**，为什么？

采用dijkstra算法处理带有负权边的图时有可能出现这样一种情况：因为dijktra算法每一次将一个节点加入已访问集合之中，之后不能在更新，如图算法刚开始执行时，将A添加到集合中标记已访问，之后选出从A到所有节点中的最短的d，于是把C加入集合中标记已访问，之后C不能在更新了，而显然，A与C之间最短路径权值为0（A-B-C），发生错误。

![img](https://img-blog.csdn.net/20140527115318953?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmxpbmtfaW52b2tlcg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)



# dijkstra算法，真的不能计算负权值吗？

https://blog.csdn.net/highlighters/article/details/119104535?



[(22条消息) 图的几种存储方式_追风者-CSDN博客_图的存储](https://blog.csdn.net/weixin_43721423/article/details/86681572)





求解过程

一：朴素迪杰斯特拉算法（枚举）

1.存图，用领接矩阵

2.初始化，包括初始化源点到每个点的距离数组dest，标记数组（在选入子集中用到）

​    一开始只知道源点到源点的距离，其他的定义为inf

3.更新入选子集（没标记的才能选），选离子集最短的

4.更新子集到其他点的距离



二：用小根堆堆优化的迪杰斯特拉算法

1.存图，用vector形式领接表

2.初始化，包括初始化源点到每个点的距离数组dest，小根堆的初始化（放入到源点的距离）

​    小根堆中值的含义是源点到其他点的最低按距离，一开始里自身最短，故开始放入到源点的距离

3.更新选入子集，堆顶的就是新的离子集最短

4.更新子集到其他点的距离





#### [743. 网络延迟时间](https://leetcode-cn.com/problems/network-delay-time/)

```c++
/*
首先，Dijkstra 算法需要存储各个边权，由于本题节点数量不超过 100100，所以代码中使用了邻接矩阵 g[i][j] 存储从点 i 到点 j 的距离。若两点之间没有给出有向边，则初始化为 inf。算法还需要记录所有点到源点的最短距离，代码中使用了 dist[i] 数组存储源点到点 i 的最短距离，初始值也全部设为 inf。由于本题源点为 KK，所以该点距离设为 0。
其次，Dijkstra 算法需要标记某一节点是否已确定了最短路，在代码中使用了 used[i] 数组存储，若已确定最短距离，则值为 true，否则值为 false。
之所以 inf 设置为 INT_MAX / 2，是因为在更新最短距离的时候，要有两个距离相加，为了防止溢出 int 型，所以除以 2


*/


int networkDelayTime(vector<vector<int>>& times, int n, int k) {
    const int inf = INT_MAX / 2;


    //用领接矩阵存图
    vector<vector<int> > g(n, vector<int>(n, inf));
    for (auto& t : times) {
        int start = t[0] - 1;
        int dest = t[1] - 1;
        int cost = t[2];
        g[start][dest] = cost;
    }

    //一维数组dest[i]表示从源点到下标为i的点的距离
    vector<int> dest(n, inf);

    //初始化源点到源点的距离为零
    dest[k - 1] = 0;

    //判断是否访问
    vector<bool> used(n, 0);

    //有n个点没要更新n次，每次加入一个点后更新
    for (int i = 0; i < n; ++i) {

        //选出为加进子集的点中，离源点路径最小的点
        int x = -1;
        for (int y = 0; y < n; ++y) {
            // dest[x]为源点到x的距离
            if (!used[y] && (x == -1 || dest[y] < dest[x])) {
                x = y;  //选出点的下标为x
            }
        }

        used[x] = 1; //选出后做上标记

        //加入新的点后开始更新
        for (int y = 0; y < n; ++y) {
            /*
            dest[y]为x未加入子集前源点到未加入子集其他点的距离，dest[x] + g[x][y]为加入新点后源点到其他子集的距离
            选其中最小的
            */
            dest[y] = min(dest[y], dest[x] + g[x][y]);
        }
    }
    int res = *max_element(dest.begin(), dest.end());
    return res == inf ? -1 : res;
}

```



已这题为例 了解Dijkstra 算法：





![img](https://pic.leetcode-cn.com/1627869535-JqaTqX-image.png)



初始图源点到源点的距离为零，到其他点的距离为inf

```c++
 //一维数组dest[i]表示从源点到下标为i的点的距离
    vector<int> dest(n, inf);

    //初始化源点到源点的距离为零
    dest[k - 1] = 0;

```







![image.png](https://pic.leetcode-cn.com/1627869608-ZNeWka-image.png)





将顶点 源点 进行标识（第一轮总是从源点开始，因为只有源点初始化了），并作为点 x，更新其到其他所有点的距离。一轮循环结束， ++i。









![image.png](https://pic.leetcode-cn.com/1627869725-ywvvoW-image.png)



```c++
   //选出为加进子集的点中，离源点路径最小的点
        int x = -1;
        for (int y = 0; y < n; ++y) {
            // dest[x]为源点到x的距离
            if (!used[y] && (x == -1 || dest[y] < dest[x])) {
                x = y;  //选出点的下标为x
            }
        }

        used[x] = 1; //选出后做上标记
```







![image.png](https://pic.leetcode-cn.com/1627869851-VtwdFS-image.png)



```c++
     //加入新的点后开始更新
        for (int y = 0; y < n; ++y) {
            /*
            dest[y]为x未加入子集前源点到未加入子集其他点的距离，dest[x] + g[x][y]为加入新点后源点到其他子集的距离
            选其中最小的
            */
            dest[y] = min(dest[y], dest[x] + g[x][y]);
        }
    }
```





将顶点 `2` 进行标识，并作为新的点 x*x*，更新。我们看到，原本点 `1` 的最短距离为 `5`，被更新为了 `3`。同理还更新了点 `3` 和点 `4` 的最短距离。





![image.png](https://pic.leetcode-cn.com/1627869870-IgtqGV-image.png)



![image.png](https://pic.leetcode-cn.com/1627869881-fDnltO-image.png)



将顶点 `1` 进行标识，并作为新的点 x*x*，同样更新了点 `4` 到源点的最短距离。



![image.png](https://pic.leetcode-cn.com/1627869914-XQvKqz-image.png)

再分别标识点 `4` 和点 `3`，循环结束。







用堆优化的Dijkstra 算法

步骤：

1.根据边数组建图，只存存在的边

2.建立堆（一般是小根堆，greater<>),堆有两个元素，一个权值，一个目的点，注意权值一定要重载<

3.入选顶点（堆相当于每次将要入选的顶点放到堆顶）并更新（自己实现），注意堆中存的是所有的最佳情况注意在，入选时可以先将没更新的排除

```c++
//含义：顶点数为n，time存储的边，求k为源点到其他点的最短路径
int networkDelayTime(vector<vector<int>>& times, int n, int k) {
	const int inf = INT_MAX / 2;

	//用vector形式的领接表存图，只存存在的边，g[0]存储0到其他点的终点和权值信息
	vector<vector<pair<int, int>>> g(n);
	for (auto &t : times) {
        //此题小标从1开始故而减1
		int start = t[0] - 1;
		int end = t[1] - 1;
		/* g【x】的值（pair）代表x到其他点（pair.first）的距离(pair.second) */
		g[start].emplace_back(end, t[2]);
        /*
        无向图要加上此条  
        g[end].emplace_back(start, t[2]);
        */
	}

	//初始化
	vector<int> dist(n, inf);
    //k-1是源点，到自身的距离为零
	dist[k - 1] = 0;
    //q中存的是start到其他点的最佳情况所有可能，可能有旧的
	priority_queue<pair<int, int>, vector<pair<int, int> >,greater<>> q;
	//加堆中的数是源点到其他点的距离，放入堆是要把距离作为first
	q.push({ 0, k - 1 });

	//小根堆的顶端是到其他点的最短距离，直接将堆顶点加入子集，加入子集后最短路不会改变
	while (!q.empty()) {
		auto p = q.top();
		q.pop();
		//源点到x的距离为time
		int time = p.first, x = p.second;
        
        //如果之前的更新过有短的情况那么之前的情况（旧的）可直接跳过
		if (dist[x] < time) {
			continue;
		}

		//入选x后更新,每一次能运行到这就相当于入选一个x，但每次遍历不能保证不选入已经在子集中
		for (auto& e : g[x]) {
			//入选后准备更新，点x到下标为y的点的距离为d
			int y = e.first, d = dist[x] + e.second;
            //在已经在子集中的一定满足这个条件，故这个for循环可以看做遍历子集外点判断是否更新
			if (d < dist[y]) {
				dist[y] = d;
                //把能到的情况或更新的情况（更短）都加入小根堆
				q.emplace(d, y);
			}
		}
	}
    //得到的dist数组是源点到其他点的距离，dist[0]表示到第一个点的距离
    
    
	int ans = *max_element(dist.begin(), dist.end());
	return ans == inf ? -1 : ans;
}
```





#### [5888. 网络空闲的时刻](https://leetcode-cn.com/problems/the-time-when-the-network-becomes-idle/)

此题用迪杰斯特拉要注意是一个无向无权（权值为1）题中edges只表示两个点之间有边，但没有表明方向



```c++

class Solution {
public:
int networkBecomesIdle(vector<vector<int>>& edges, vector<int>& patience) {
	const int inf = INT_MAX / 2;
	int n = patience.size();

	//用vector形式的领接表存图
	vector<vector<pair<int, int>>> g(n);
	for (auto& t : edges) {
		int start = t[0];
		int end = t[1];
		/* g【x】的值（pair）代表x到其他点（pair.first）的距离(pair.second) */
        //这里无向图转化为双向有向图
		g[start].emplace_back(end, 1);
		g[end].emplace_back(start, 1);
	}
	//初始化
	vector<int> dist(n, inf);
	dist[0] = 0;
	priority_queue<pair<int, int>, vector<pair<int, int> >, greater<>> q;
	//加堆中的数是源点到其他点的距离，放入堆是要把距离作为first
	q.push({ 0, 0 });
	//小根堆的顶端是到其他点的最短距离，直接将堆顶点加入子集
	while (!q.empty()) {
		auto p = q.top();
		q.pop();
		//源点到x的距离为time
		int time = p.first, x = p.second;


		if (dist[x] < time) {
			continue;
		}

		//入选x更新
		for (auto& e : g[x]) {
			//点x到下标为y的点的距离为d
			int y = e.first, d = dist[x] + e.second;
			if (d < dist[y]) {
				dist[y] = d;
				//把能到的点都加入小根堆
				q.emplace(d, y);
			}
		}
	}

	int ans = 0;
	for (int i = 1; i < n; ++i) {
        //一个消息走过的路程
		int stop = dist[i] * 2;
        /* 
        最后一个消息走过的路程
        第一次信号回来之前发出的信号次数:(stop - 1) / patience[i]
        那么最后一次信号的发出时间:(stop - 1) / patience[i] * patience[i]
        */
		int last_send = (stop - 1) / patience[i] * patience[i];
		ans = max(ans, last_send + stop);
	}
	return ans + 1;
}	
};
```



#### [1514. 概率最大的路径](https://leetcode-cn.com/problems/path-with-maximum-probability/)

此题堆迪杰斯特拉算法的修改：1.大根堆改为小根堆  2.距离变为概率的乘积 3.距离值的类型为double 4.初始化距离



```c++
double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start, int end) {
	//用vector形式的领接表存图
	vector<vector<pair<int, double>>> g(n);
	int i = 0;
	for (auto& t : edges) {
		int start = t[0];
		int end = t[1];
		/* g【x】的值（pair）代表x到其他点（pair.first）的距离(pair.second) */
		//这里无向图转化为双向有向图
		g[start].emplace_back(end, succProb[i]);
		g[end].emplace_back(start, succProb[i]);
		++i;
	}

	//初始化，一开始不能到达其他点概率为0
	vector<double> dist(n, 0);
    //源点到自身的概率为1
	dist[start] = 1;
	priority_queue<pair<double, int>, vector<pair<double, int> >, less<>> q;
	//加堆中的数是源点到其他点的距离，放入堆是要把距离作为first
	q.push({ 1, start });
	//大根堆的顶端是到其他点的最短距离，直接将堆顶点加入子集
	while (!q.empty()) {
		auto p = q.top();
		q.pop();
		//源点到x的距离为time
		double time = p.first;
		int x = p.second;


		//入选x更新
		for (auto& e : g[x]) {
			//点x到下标为y的点的距离为d
			int y = e.first;
			double d = dist[x] * e.second;
		    
            //如果找到概率更大的
			if (d > dist[y]) {
				dist[y] = d;
				//把能到的点都加入大根堆
				q.emplace(d, y);
			}
		}
	}

	return dist[end];
}
```

#### [976. 到达目的地的方案数](https://leetcode-cn.com/problems/number-of-ways-to-arrive-at-destination/)

计算最短路的同时进行最短路数量的统计

```c++
int countPaths(int n, vector<vector<int>>& roads) {
		const int mod = 1e9 + 7;
		int ans = 0;

		const long long inf = LLONG_MAX / 2;

		//用vector形式的领接表存图
		vector<vector<pair<int, int>>> g(n);
		vector<long long> cnt(n);
		cnt[0] = 1;
		for (auto& t : roads) {
			//此题小标从1开始故而减1
			int start = t[0];
			int end = t[1];
			/* g【x】的值（pair）代表x到其他点（pair.first）的距离(pair.second) */
			g[start].emplace_back(end, t[2]);

			g[end].emplace_back(start, t[2]);
		}
		//初始化
		vector<long long> dist(n, inf);
		//k-1是源点，到自身的距离为零
		dist[0] = 0;
		priority_queue<pair<long long, int>, vector<pair<long long, int> >, greater<>> q;
		//加堆中的数是源点到其他点的距离，放入堆是要把距离作为first
		q.push({ 0, 0 });

		//小根堆的顶端是到其他点的最短距离，直接将堆顶点加入子集
		while (!q.empty()) {
			auto p = q.top();
			q.pop();
			//源点到x的距离为time
			long long time = p.first;
            int x = p.second;


			if (dist[x] < time) {
				continue;
			}
            
            
            /*
            x已经入选，最短路以及数量不会再改变
            三种情况：
            入选x后路径变短d < dist[y]，到y的最短路数量等于到x的最短路数量
            变长d < dist[y]
            相等d == dist[y]，到y的最短路数量等于所有满足条件x（d == dist[x] + e.second）最短路数量之和
            */
            
			//入选x后更新
			for (auto& e : g[x]) {
				//点x到下标为y的点的距离为d
				int y = e.first;
                long long d = dist[x] + e.second;
				
				if (d < dist[y]) {
					cnt[y] = cnt[x];
   
					dist[y] = d;
					//把能到的点都加入小根堆
					q.emplace(d, y);
				}
				else if (d == dist[y]) {
					cnt[y] = cnt[y] + cnt[x];
				}
				
			}
		}
		//得到的dist数组是源点到其他点的距离，dist[0]表示到第一个点的距离
		return cnt[n- 1] % mod;
	}
```





时间复杂度：O(mlogm)，其中 m 是图中边的数量。如果不使用任何优化，时间复杂度是 O(mn)，其中 n 是图中点的数量。使用不同的数据结构优化，将会表现出不同的时间复杂度：

优先队列（例如 C++ 中的 priority_queue）优化：O(mlogm)；
手写堆优化：O(mlogn)；
线段树优化：O(mlogn)；
斐波那契堆优化：O(nlogn+m)。
空间复杂度：O(m)，其中 m 是图中边的数量。









旅游规划 

有了一张自驾旅游路线图，你会知道城市间的高速公路长度、以及该公路要收取的过路费。现在需要你写一个程序，帮助前来咨询的游客找一条出发地和目的地之间的最短路径。如果有若干条路径都是最短的，那么需要输出最便宜的一条路径。

### 输入格式:

输入说明：输入数据的第1行给出4个正整数*N*、*M*、*S*、*D*，其中*N*（2≤*N*≤500）是城市的个数，顺便假设城市的编号为0~(*N*−1)；*M*是高速公路的条数；*S*是出发地的城市编号；*D*是目的地的城市编号。随后的*M*行中，每行给出一条高速公路的信息，分别是：城市1、城市2、高速公路长度、收费额，中间用空格分开，数字均为整数且不超过500。输入保证解的存在。

### 输出格式:

在一行里输出路径的长度和收费总额，数字间以空格分隔，输出结尾不能有多余空格。

### 输入样例:

```in
4 5 0 3
0 1 1 20
1 3 2 30
0 3 4 10
0 2 2 20
2 3 1 20
```

### 输出样例:

```out
3 40
```







```c++
#include<iostream>
#include<vector>
#include<queue>
#include<algorithm>
#include<numeric>
#include<set>
#include<unordered_set>
#include<string>
#include<unordered_map>
#include <iomanip>
#include<map>
using namespace std;


//权值，注意重载<
struct Value {
	int x;
	int price;
	bool operator<(const Value& a) const { return x < a.x; }
};




Value Dijkstra(vector<vector<int> >& edge, int n, int start, int end) {
	const int inf = INT_MAX / 2;
   //建图
	vector < vector<pair<int, Value>> > g(n);
	for (auto&u : edge) {
		int start = u[0], end = u[1];
		Value value;
		value.x = u[2], value.price = u[3];
		g[start].emplace_back(end, value);
		g[end].emplace_back(start, value);
	}

	priority_queue<pair<Value, int>, vector<pair<Value, int>>, greater<>> q;

	vector<Value> dist(n);
	for (int i = 0; i < n; ++i) {
		dist[i].x = dist[i].price = inf;
	}
	dist[start].x = dist[start].price = 0;

	q.push({ Value { 0, 0 }, start });
    
    
	while (!q.empty()) {
		auto p = q.top();

		q.pop();
		int a = p.second, x = p.first.x, price = p.first.price;

		if (x > dist[a].x) {
			continue;
		}
		else if (x == dist[a].x) {
			if (price > dist[a].price) {
				continue;
			}
		}

		for (auto& u : g[a]) {
			int b = u.first, y = u.second.x + dist[a].x, price = u.second.price + dist[a].price;
            
            
			if (dist[b].x > y) {
				dist[b].x = y, dist[b].price = price;
				Value tmp = { dist[b].x ,dist[b].price };
				q.push({tmp, b});
			}
			//此题的重点，当路径相同时选价格最便宜的
			else if (dist[b].x == y) {
				if (dist[b].price > price) {
					dist[b].price = price;
					Value tmp = { dist[b].x ,dist[b].price };
					q.push({ tmp, b });
				}
			}
		}
	}
	return dist[end];
}

int main() {
	int n, m, s, d;
	cin >> n >> m >> s >> d;
	vector<vector<int> > edge(m);
	for (int i = 0; i < m; ++i) {
		int a, b, x, p;
		cin >> a >> b >> x >> p;
		edge[i] = { a, b, x, p };
	}
	
	if (n == 2) {
		cout << edge[0][2] << " " << edge[0][3] << endl;
		return 0;
	}


	Value res = Dijkstra(edge, n, s, d);
	cout << res.x << " " << res.price << endl;
}

```

