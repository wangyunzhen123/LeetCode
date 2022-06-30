#### [797. 所有可能的路径](https://leetcode-cn.com/problems/all-paths-from-source-to-target/)

当我们决策到第 x 位（非零）时，该位置所能放入的数值由第 x−1 位已经填入的数所决定，同时由于给定的 graph 为有向无环图（拓扑图），因此按照第 x−1 位置的值去决策第 x 位的内容，必然不会决策到已经在当前答案的数值，否则会与 graph 为有向无环图（拓扑图）的先决条件冲突。

换句话说，与一般的爆搜不同的是，我们不再需要 vis数组来记录某个点是否已经在当前答案中



```c++
class Solution {

private:
    vector<vector<int>> res;
    vector<int> stk;

public:
    void dfs(vector<vector<int>>& graph, int start, int end) {
        if (start == end) {
            res.push_back(stk);
            return;
        }
        
        for (auto s : graph[start]) {
            stk.push_back(s);
            dfs(graph, s, end);
            //回溯
            stk.pop_back();
         }
        
 }


    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        stk.push_back(0);
        dfs(graph, 0, graph.size() - 1);
        return res;
    }
};

```





#### [1992. 找到所有的农场组](https://leetcode-cn.com/problems/find-all-groups-of-farmland/)

```c++
class Solution {
    vector<vector<int> > res;
    vector<int> point;

public:

    void dfs(vector<vector<int>>& land, int i, int j) {
       if(i < 0 || j < 0 || i >= land.size() || j >= land[0].size() || land[i][j] == 0) {
           return;
       }
       land[i][j] = 0;
       point[2] = max(point[2], i);
       point[3] = max(point[3], j);
       dfs(land, i + 1, j);
       dfs(land, i, j + 1);
       dfs(land, i - 1, j);
       dfs(land, i, j - 1);
    }

    vector<vector<int>> findFarmland(vector<vector<int>>& land) {
        for (int i = 0; i < land.size(); ++i) {
            for (int j = 0; j < land[0].size(); ++j) {
                if (land[i][j] == 1) {
                    point = {i , j , -1,  -1} ;
                    dfs(land, i, j);
                     res.push_back(point);
                }
            }
        }
        return res;
    }
};
```



#### [212. 单词搜索 II](https://leetcode-cn.com/problems/word-search-ii/)

上面两个遍历只要遍历到就行，不讲究顺序，而此题不仅要遍历到遍历的先后顺序对结果也有影响，因此四周都要遍历

```c++
class Solution {

	string str;
	vector<string> res;
	unordered_set<string> set;
    vector<vector<char>> _board;
    vector<string>  _words;
	vector < vector<int> > dir = {{1,0},{-1,0},{0,1},{0,-1}};
    vector<vector<int> > visit;

public:
	void dfs(string& str, int r, int c) {
        //题目要求str的长度不超过10
        if(str.size() > 10) {
            return;
        }
        
        //找到后加入结果集合并从set中删除
        if (set.find(str) != set.end()) {
            res.push_back(str);
            set.erase(str);
        }



        for(auto d : dir) {
            int dx = d[0] + r, dy = d[1] + c;

            if(dx < 0 || dx >= _board.size() || dy < 0 || dy >= _board[0].size() 
            || visit[dx][dy]) 
            continue;

            visit[dx][dy] = 1;
            str.push_back(_board[dx][dy]);
            dfs(str, dx, dy);
            
            //不满足条件后或走到底后回溯
            visit[dx][dy] = 0;
            str.pop_back();
        }
	}



	vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        _board = board;
        _words = words;
		int row = board.size();
		int col = board[0].size();
        visit.resize(row, vector<int>(col, 0));

		for (int i = 0; i < words.size(); ++i) {
			set.insert(words[i]);
		}
		

		for (int r = 0; r < row; ++r) {
			for (int c = 0; c < col; ++c) {
                visit[r][c] = 1;
                str.push_back(_board[r][c]);
				dfs(str, r, c);
                visit[r][c] = 0;
                str.pop_back();
			}
		}
		return res;
	}
};

```









# CCF201709-4 通信网络（[(52条消息) CCF201709-4 通信网络（100分）【DFS+BFS】_海岛Blog-CSDN博客](https://tigerisland.blog.csdn.net/article/details/119796010)）

**问题分析**：

图连通性问题：[(52条消息) 图论 —— 图的连通性_Alex_McAvoy的博客-CSDN博客_图的连通性](https://blog.csdn.net/u011815404/article/details/83217012)

对于无向图，对其进行遍历时：
◆ 若是连通图：仅需从图中任一顶点出发，就能访问图中的所有顶点；
◆ 若是非连通图：需从图中多个顶点出发。每次从一个新顶点出发所访问的顶点集序列恰好是各个连通分量的顶点集；

如图所示的无向图是非连通图，按图中给定的邻接表进行深度优先搜索遍历，2次调用DFS所得到的顶点访问序列集是： { v1 ,v3 ,v2}和{ v4 ,v5 }

也就是说连通图遍历一次就能访问所有顶点

![preview](https://pic3.zhimg.com/v2-306ba9f650757c8ad563ff59ec914cc6_r.jpg)

看成无向图构造反图：

无向图从一个点经过若干个点到达，另一个点反过来也是

```c++
#include<bits/stdc++.h>
using namespace std;

const int N = 1000 + 1;
vector<vector<int> > g1;
vector<vector<int> > g2;
bool vis1[N], vis2[N];


void dfs1(int x) {
	vis1[x] = true;
	for (auto u : g1[x]) {
		if(!vis1[u])
		dfs1(u);
	}
}

void dfs2(int x) {

	vis2[x] = true;
	for (auto u : g2[x]) {
		if (!vis2[u])
		dfs2(u);
	}
}

int main(int argc, char** argv) {
	int N, M;
	cin >> N >> M;
	g1.resize(N + 1);
	g2.resize(N + 1);

	for (int i = 0; i < M; ++i) {
		int start, end;
		cin >> start >> end;
		g1[start].push_back(end);
		g2[end].push_back(start);
	}


	int ans = 0;
	for (int i = 1; i <= N; ++i) {
		
		memset(vis1, false, sizeof (vis1));
		dfs1(i);
		memset(vis2, false, sizeof (vis2));
		dfs2(i);

		bool flag = 1;
		for (int j = 1; j <= N; ++j) {
            //能访问所有顶点
			if (!vis1[j] && !vis2[j]) {
				flag = 0;
				break;
			}
		}
		if (flag) {
			++ans;
		}
	}
	cout << ans << endl;
}
```







# 欧拉回路

[(60条消息) PAT 7-5 哥尼斯堡的“七桥问题” （25 分）（解题报告）_Tracker-CSDN博客](https://blog.csdn.net/qq_40099908/article/details/83048098)

原理及定义：

欧拉回路：图G，若存在一条路，经过G中每条边有且仅有一次，称这条路为欧拉路，如果存在一条回路经过G每条边有且仅有一次，

称这条回路为欧拉回路。具有欧拉回路的图成为欧拉图。

判断欧拉路是否存在的方法

有向图：图连通，有一个顶点出度大入度1，有一个顶点入度大出度1，其余都是出度=入度。

无向图：图连通，只有两个顶点是奇数度，其余都是偶数度的。

判断欧拉回路是否存在的方法

有向图：图连通，所有的顶点出度=入度。

无向图：图连通，所有顶点都是偶数度。

 

 1．凡是由偶点组成的连通图，一定可以一笔画成。

 2．凡是只有两个奇点的连通图（其余都为偶点），一定可以一笔画成。

3．其他情况的图都不能一笔画出。

```c++
#include <iostream>
#include<vector>

using namespace std;
/* run this program using the console pauser or add your own getch, system("pause") or input loop */



void dfs(vector<vector<int> >& g, vector<bool>& vis, int i) {
	if (vis[i]) return;
	vis[i] = true;
	for (auto t : g[i]) {
		if (vis[t]) continue;
		dfs(g, vis, t);
	}
}

int main(int argc, char** argv) {
	int n, m;
	cin >> n >> m;
	vector<vector<int> > g(n, vector<int>(n));
	vector<int> ind(n, 0);
	vector<bool> vis(n, false);
	for (int i = 0; i < m; ++i) {
		int start, end;
		cin >> start >> end;
		g[start - 1].push_back(end - 1);
		g[end - 1].push_back(start - 1);
		++ind[start - 1];
		++ind[end - 1];
	}
	dfs(g, vis, 0);

	bool flag = true;
	for (int i = 0; i < n; ++i) {
        //若干有个点的度数不为偶数或有个点未被访问（不连通），则一定不是欧拉回路
		if (ind[i] % 2 != 0 || vis[i] == false) {
			flag = false;
			break;
		}
	}

	cout << flag << endl;
}
```

