#### [1334. 阈值距离内邻居最少的城市](https://leetcode-cn.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/)

```c++
int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
    const int inf = INT_MAX / 2;
    
    //存图：邻接矩阵，注意初始化距离不可达
	vector<vector<int> > f(n, vector<int>(n, inf));
    
	for (auto t : edges) {
		int start = t[0];
		int end = t[1];
		int cost = t[2];
		f[start][end] = cost;
		f[end][start] = cost;
	}
    
    //Floyd算法，得到一个f数组,f[x][y]表示x到y的最短距离，可以得到所有点之间的最短距离
    	for (int k = 0; k < n; k++) {
		for (int x = 0; x < n; x++) {
			for (int y = 0; y < n; y++) {
				f[x][y] = min(f[x][y], f[x][k] + f[k][y]);
			}
		}
	}
    
    
	int idx = -1;
	int mincnt = INT_MAX;
	for (int x = 0; x < n; ++x) {
		int count = 0;
		for (int y = 0; y < n; ++y) {
			if (f[x][y] <= distanceThreshold && x != y) {
				count++;
			}
		}
		if (count <= mincnt) {
			mincnt = count;
			idx = x;
		}
	}
	return idx;
}
```

