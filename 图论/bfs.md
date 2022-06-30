#### [5916. 转化数字的最小运算数](https://leetcode-cn.com/problems/minimum-operations-to-convert-number/)

典型的无权最短路问题，我们用BFS求解。

每种运算相当于走一条路，queue存储当前的数和次数



时间复杂度O(NC)，其中CC表示有效数值的范围大小。
空间复杂度O(C)。



```c++
int minimumOperations(vector<int>& nums, int start, int goal) {
	queue<pair<int, int> > q;
	q.emplace(start, 0);
    
	vector<bool> vis(1005, 0);
	vis[start] = true;

	int n = nums.size();
	while (!q.empty()) {
		int x = q.front().first;
		int time = q.front().second;
		q.pop();

		if (x == goal) return time;

		for (int i = 0; i < nums.size(); ++i) {
			int add = x + nums[i], minus = x - nums[i], xo = x ^ nums[i];

			//加
			if (add < 0 || add > 1000) {
				if (add == goal) {
					return time + 1;
				}
			}
			else if(!vis[add]){
				vis[add] = true;
				q.emplace(add, time + 1);
			}

			//减
			if (minus < 0 || minus > 1000) {
				if (minus == goal) {
					return time + 1;
				}
			}
			else if (!vis[minus]) {
				vis[minus] = true;
				q.emplace(minus, time + 1);
			}


			//异或
			if (xo < 0 || xo > 1000) {
				if (xo == goal) {
					return time + 1;
				}
			}
			else if (!vis[xo]) {
				vis[xo] = true;
				q.emplace(xo, time + 1);
			}
		}
	}
	return -1;
}
```







**Wiring problem**

[题目详情 - 7-2 Wiring problem (Algorithm A&D) (50 分) (pintia.cn)](https://pintia.cn/problem-sets/1452569798811336704/problems/1452569855900008449)

```c++
#include <iostream>
#include<map>
#include<unordered_map>
#include<vector>
#include<algorithm>
#include<queue>
using namespace std;
/* run this program using the console pauser or add your own getch, system("pause") or input loop */

struct Node {
	int x;
	int y;
	int step;
};

int dir[4][2] = { {0,1}, {0,-1}, {1, 0}, {-1,0} };


int main() {
	int n, m;
	cin >> n >> m;
	vector<vector<int> > grid(n, vector<int>(m));
	vector<vector<bool> > vis(n, vector<bool>(m));
	for (int i = 0; i < n; ++i) {
		for (int j = 0; j < m; ++j) {
			cin >> grid[i][j];
		}
	}

	int x1, y1, x2, y2;
	cin >> x1 >> y1 >> x2 >> y2;

	int step = 0;
	queue<Node> q;
	q.push({ x1, y1, step });
	vis[x1][y1] = true;
	bool flag = 0;
	while (!q.empty()) {
		Node tep = q.front();
		q.pop();
		if (tep.x == x2 && tep.y == y2) {
			step = tep.step;
			flag = 1;
			break;
		}
		
		
		for (int i = 0; i < 4; ++i) {
			if (tep.x + dir[i][0] >= 0 && tep.x + dir[i][0] < n &&
				tep.y + dir[i][1] >= 0 && tep.y + dir[i][1] < m) {
				if (grid[tep.x + dir[i][0]][tep.y + dir[i][1]] == 0 &&
					vis[tep.x + dir[i][0]][tep.y + dir[i][1]] == false) {

					q.push({ tep.x + dir[i][0], tep.y + dir[i][1], tep.step + 1 });
					vis[tep.x + dir[i][0]][tep.y + dir[i][1]] = true;
				}
			}
		}	
	}



	if(flag)
	cout << step << endl;
	else {
		cout << 0 << endl;
	}
}
```

