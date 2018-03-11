---
layout: post
title: Gragh practice
categories:
- Algorithm
tags:
- bfs
- graph
date : "2016-10-12T00:00:00"
---


# Graph

Leetcode 里现有的图论的题比较简单，都有套路。八个题七个BFS一个DFS。

后半部分把图里的搜索的题目也放了进来。

BFS Direct Graphs - Topological Sorting

Course Schedule

用两个个hashmap记录所有的入度和出度。

用一个zeroInDegree的队列做BFS，没有入度意味着起点

在BFS过程中，不断的删除孩子的入度，然后把父亲节点从出度中删掉，这样就把这个点和所有和他连接的孩子的边都删掉了。

最后判断如果出度中还有点，说明还有边存在着。

```cpp
//BFS
class Solution {
	public:
		bool canFinish(int numCourses, vector<pair<int, int>>& pres) {
			queue<int> zeroInDegree;

			unordered_map<int, unordered_set<int>> inDegree;
			unordered_map<int, unordered_set<int>> outDegree;

			for (int i = 0; i < pres.size(); i++) {
				inDegree[pres[i].first].emplace(pres[i].second);
				outDegree[pres[i].second].emplace(pres[i].first);
			}

			for (int i = 0; i < numCourses; i++) {
				if (!inDegree.count(i)) {
					zeroInDegree.push(i);
				}
			}

			while (!zeroInDegree.empty()) {
				int parent = zeroInDegree.front();
				zeroInDegree.pop();
				//for each child has a edge from parent
				for (auto child : outDegree[parent]) {
					//remove edge
					inDegree[child].erase(parent);
					//if this child has no edge, add to queue
					if (inDegree[child].empty()) {
						zeroInDegree.push(child);
					}
				}
				outDegree.erase(parent);
			}

			//if still exist edges in the graph, return false
			if (!outDegree.empty()) {
				return false;
			}
			return true;   
		}
};

```


Course Schedule II

BFS的过程中顺便记录弹出的顺序就行了。

Alien Dictionary

每个字母就是一个节点，根据排好序的词，找到第一个不想等的字母，找到关系，建立direct graph.

剩下的跟 Course Schedule ii 无异，BFS就行。

BFS Undirected Graphs

Clone Graph

用一个hashmap来记录orignal graph 与 copied graph 之间的节点的一一对应的关系。

用BFS遍历原图。做两件事：加没有遇到过的点到copied graph里，把coped graph里的节点按照 orignal graph 的连接关系连接－也就是加到neighbor里面。

Graph Valid Tree

給一堆边来表示graph，问是不是valid tree。区别就在于 树里面 不可能有环。 那么如何探测图里面的环？遍历一下就行，如果是树，那么每个点只会被访问一次，如果有环，则会被访问多次。

Undirected graph 没有入度和出度一说，所有的边都是度。第一步建图，只需一个map, 记录点和对应的边。

然后从任意点开始做bfs，要用一个set或者vector来记录访问过的点，再次访问的话就判断不是树。

弹出一个父亲：在孩子节点上删除父亲到孩子的边，将孩子压入队列，然后删除该父亲。

最后要加一个判断，看是否所有的点都访问过。不然有可能是两个不连接的图。那也不符合题意。

Number of Connected Components in an Undirected Graph

需要有一个visited 的数组来记录是否被访问过。遍历每一个点，对每一个没访问过的点(还存在在只由条件给的edge构造的graph中) 做BFS。BFS过程中删点和边，同时记录访问。BFS完 counter 加一。 最后要注意，因为条件给的是边，所以有的单个的点可以不在这个边的集合上，所以要再遍历一遍visited看看没有被访问过的就是单个的点。单个的点也是graph啊。这就是这个visited的意义所在。

Minimum Height Trees

最多最多只有两个 MHT 在图里。这道题才真正需要degree。首先需要明白，最大的MHT只能是两个node。证明的话用反证法，假设有3个，那么必然可以变到1个。

那么就可以用这么一个做法，每次把叶子都剥下来，直到剩下的节点数小于等于2。用degree来记录每个点的度数。那么度数为1就是叶子节点。把叶子剥下来就意味着度数为0，但是要区别最后剩下的点而不是不要的点，我们把这些剥下来的点度数设为-1。然后总数-1。对他们的父节点也要减掉度数。循环直到剩下的点小于等于2。

再用一次循环找到度数为0 或者 为 1的点，那么就是剩下来的结果。

DFS

Reconstruct Itinerary

一笔画问题。注意要用一个hashmap来表示graph，虽然这个graph是有向的，但是不需要入度出度。key是string, value需要一个multiset 因为同一个出发地可能有好几张同一个目的地的机票。

用一个while 循环对当前的点做DFS。每到一个点就往所有的孩子递归，每一次删除对应的边，直到当前节点已经没有出去的边。跳出循环后加入该点－意味着没边可以走了，就开始往回弹栈，把点压入答案中。

BFS Shortest distance

图给的是基本上邻接表，所以最短路径本来用dijkstra做的可以简化为BFS，因为路径间的权重都是1。

Walls and Gates

最基础的BFS找路径。



Word Ladder ／ Word Ladder II（TODO）

可以抽象成图的问题。难点在时间复杂度的优化上。

如何找到符合要求的neighbor？

遍历在dict里其余所有word，和当前的比较，那么是O(nk)的复杂度，算上BFS，总的复杂度能达到O(n^2k)。

k是word的长度。

依次替换当前word里的字母，找dict里是否存在，在替换回来。那么是O(26*k)的复杂度，总的复杂度可以优化到O(nk).

如何求出具体路径？用一个hashmap来记录每层的word，key是从头开始的距离，value是所有这些距离的词。然后做DFS即可。注意用一个visited去重。

还能优化吗？TODO：双端BFS。

Surrounded Regions

标记法。

## Search In Gragh

### Leetcode 317: Shortest Distance from All Buildings

找到一个点，从他开始到所有的buildings的距离最短的和，中间有障碍物。对每个点做BFS，然后加起来求一个最小值。这样的时间复杂度是 $O(m*n)[BFS] * O(m*n)[matrix] = O(m^2*n^2)$。优化：从building开始搜。那么时间复杂度为$O(k*m*n)$。 $k$ 是 building 的个数。

```cpp
class Solution {
public:
    int shortestDistance(vector<vector<int>>& grid) {
        int res = INT_MAX;
        
        vector<vector<pair<int, int>>> dis(grid.size(), vector<pair<int, int>>(grid[0].size()));    
        //dis[i][j].first => total distance from k buildings to grid[i][j]
        //dis[i][j].second => num of times search from k buildings and visited to grid[i][j] successfully (avoid dead end)
        int m = grid.size();
        int n = grid[0].size();
        int num_buildings = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    bfs(grid, dis, i, j);
                    num_buildings++;
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (dis[i][j].second == num_buildings) {
                    res = min(res, dis[i][j].first);
                }
            }
        }
        return res == INT_MAX? -1 : res;
    }
    
    
    void bfs(const vector<vector<int>> & grid, vector<vector<pair<int, int>>> & dis, int i, int j) {
        queue<pair<int, int>> q;
        q.emplace(i, j);
        int m = grid.size();
        int n = grid[0].size();
        deque<deque<bool>> visited(m, deque<bool>(n, false));
        
        vector<pair<int, int>> dirs = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
        
        int level = 0;      //distance to building in grid[i][j]
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                int x = q.front().first;
                int y = q.front().second;
                q.pop();
                if (level != 0) {
                    dis[x][y].first += level;
                    dis[x][y].second++;
                }
                for (const auto & dir : dirs) {
                    int x_prime = x + dir.first;
                    int y_prime = y + dir.second;
                    if (x_prime < m && x_prime >= 0 && y_prime < n && y_prime >= 0 && 
                        grid[x_prime][y_prime] == 0 &&
                        visited[x_prime][y_prime] == false) {
                        q.emplace(x_prime, y_prime);
                        visited[x_prime][y_prime] = true;
                    }
                }    
            }            
            level++;
        }
        
    }
};
```
Best Meeting Point

和上题一样的做法，只是可以在人所在的位置。做BFS标记visited的时候要注意。
```cpp
class Solution {
public:
    int minTotalDistance(vector<vector<int>>& grid) {
        int res = INT_MAX;        
        vector<vector<int>> dis(grid.size(), vector<int>(grid[0].size()));    
        int m = grid.size();
        int n = grid[0].size();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    bfs(grid, dis, i, j);
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                    res = min(res, dis[i][j]);
            }
        }
        return res == INT_MAX? -1 : res;        
    }
    void bfs(const vector<vector<int>> & grid, vector<vector<int>> & dis, int i, int j) {
        queue<pair<int, int>> q;
        q.emplace(i, j);
        int m = grid.size();
        int n = grid[0].size();
        deque<deque<bool>> visited(m, deque<bool>(n, false));
        visited[i][j] = true;
        vector<pair<int, int>> dirs = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
        
        int level = 0;      //distance to grid[i][j]
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                int x = q.front().first;
                int y = q.front().second;
                q.pop();
                if (level != 0) {
                    dis[x][y] += level;
                }
                for (const auto & dir : dirs) {
                    int x_prime = x + dir.first;
                    int y_prime = y + dir.second;
                    if (x_prime < m && x_prime >= 0 && y_prime < n && y_prime >= 0 && 
                        visited[x_prime][y_prime] == false) {
                        q.emplace(x_prime, y_prime);
                        visited[x_prime][y_prime] = true;
                    }
                }    
            }            
            level++;
        }
    }    
};
```
但超时啦。
答案里给的方法是算出median。并不适用有obstacle的情况（是嘛？）。

```cpp
// Time:  O(mn)
// Space: O(m+n)

class Solution {
public:
    int minTotalDistance(vector<vector<int>>& grid) {
        vector<int> x, y;
        for (int i = 0; i < grid.size(); ++i) {
            for (int j = 0; j < grid[0].size(); ++j) {
                if (grid[i][j]) {
                    x.emplace_back(i);
                    y.emplace_back(j);
                }
            }
        }
        nth_element(x.begin(), x.begin() + x.size() / 2, x.end());
        nth_element(y.begin(), y.begin() + y.size() / 2, y.end());
        const int mid_x = x[x.size() / 2];
        const int mid_y = y[y.size() / 2];
        int sum = 0;
        for (int i = 0; i < grid.size(); ++i) {
            for (int j = 0; j < grid[0].size(); ++j) {
                if (grid[i][j]) {
                    sum += abs(mid_x - i) + abs(mid_y - j);
                }
            }
        }
        return sum;
    }
};
```

[数学证明参考链接](https://math.stackexchange.com/questions/113270/the-median-minimizes-the-sum-of-absolute-deviations)

- nth_element()
把按 comparator 排序的有第n个数放在 n 的位置，前面的都比它“小”， 后面的都比它“大”。 但其他并不保证有序，时间复杂度 $O(n)$ 比 sort 好一些。

### Leetcode 407: Trapping Rain Water II
```cpp
class Solution {
public:
    int trapRainWater(vector<vector<int>>& heightMap) {
        if (heightMap.empty()) {
            return 0;
        }
        int rst = 0;
        int m = heightMap.size();
        int n = heightMap[0].size();

        auto cmp = [&heightMap](const pair<int, int> & a, const pair<int, int> & b) {
            return heightMap[a.first][a.second] > heightMap[b.first][b.second]; 
        };
        
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> min_heap(cmp);
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        vector<pair<int, int>> dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        
        //start with boundary
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if(!(i==0 || i==m-1 || j==0 || j==n-1)) continue;
                min_heap.emplace(i, j);
                visited[i][j] = 1;
            }
        }
        
        int max_h = 0;
        int x, y;
        while (!min_heap.empty()) {
            x = min_heap.top().first;
            y = min_heap.top().second;
            max_h = max(heightMap[x][y], max_h);
            min_heap.pop();
            for (auto dir : dirs) {
              int x_n = x + dir.first;
              int y_n = y + dir.second;
              if (x_n > 0 && x_n < m - 1 && y_n > 0 && y_n < n - 1 && !visited[x_n][y_n]) {
                visited[x_n][y_n] = true;
                rst += max(0, (max_h - heightMap[x_n][y_n]));
                min_heap.emplace(x_n, y_n);
              }              
            }
        }
        return rst;
    }
};
```

Skyline