[参考灵神写法](https://leetcode.cn/problems/network-delay-time/solutions/2668220/liang-chong-dijkstra-xie-fa-fu-ti-dan-py-ooe8/)
**Dijkstra**算法适用于求中从单个源节点出发求所有源节点到目标节点的最短距离。节点之间的边既可以是有向边也可以是无向边。（且节点之间的权重是非负数）。
其核心是维护一个**minDist**数组，数组的下标对应目标节点编号，算法需要时刻维护minDist数组的定义。（循环不变量）。**Dijkstra**算法是一种贪心思想，遍历当前的minDist数组，找到未建立最短路径节点的点s，从中选取路径最小的点。然后利用该点更新**minDist**数组。
### 写法1：朴素Dijkstra（适用于稠密图，时间复杂度与节点个数相关）
```cpp
class Solution{
public:
    vector<int> Dijkstra(const vector<vector<int>> &weights, int n, int k){
        /*
        输入：
            weights:二维矩阵存放0 ~ n - 1节点之间的边的权重。weights[i] = (u, v, w)
            n:代表节点个数
            k:代表源节点
        输出：
            minDist数组
        */
        /* 创建邻接矩阵 */
        vector<vector<int>> grid(n, vector<int>(n, INT_MAX / 2)); // INT_MAX / 2是防止溢出

        for (const auto &wweight : weights){
            int u = weight[0], v = weight[1], w = weight[2];
            grid[u][v] = grid[v][u] = w;
        }

        vector<int> minDist(n, INT_MAX / 2), done(n); // done数组记录当前节点是否已经加入结果中
        // 初始化
        minDist[k] = 0;
        while (true){
            // 1. 遍历minDist数组，在非done中选择距离最短的节点
            int x = -1; // 当前循环中源节点到x节点的距离最短
            for (int i = 0; i < n; ++i){
                if (!done[i] && (x < 0 || minDist[i] < minDist[x])){
                    x = i;
                }
            }
            if (x < 0 || minDist[x] == INT_MAX / 2){
                // x < 0说明所有节点都已经加入到结果集当中
                // minDist[x] == INT_MAX / 2说明目前到源点最近的节点已经是不可达的了，可以直接结束循环。
                return minDist;
            }
            // 2. 将x加入到结果集中
            done[x] = 1;
            // 3. 利用x更新minDist数组
            for (int i = 0; i < n; ++i){
                if (grid[x][i] != INT_MAX / 2){
                    minDist[i] = min(minDist[x] + grid[x][i], minDist[i]);
                }
            }
        }
    }
};
```
**复杂度分析**
- 时间复杂度分析：O(n<sup>2</sup>)
- 空间复杂度分析：O(n<sup>2</sup>)
### 写法2：堆优化版Dijkstra（适用于稀疏图，因为时间复杂度与边的数量相关）
```cpp
class Solution{
public:
    vector<int> Dijkstra(const vector<vector<int>> &weights, int n, int k){
        /*
        输入
            weights:二维矩阵存放0 ~ n - 1节点之间的边的权重, weights[i] = (u, v, w);
            n:代表节点个数
            k:代表源节点
        输出：
            minDist矩阵
        */

       // 创建邻接表存储边
       // pair<int, int> 第一个int代表边的权重，第二个int代表与from节点连接的to节点
       vector<vector<pair<int, int>>> edges(n);
       for (const auto &weight : weights){
            int u = weight[0], v = weight[1], w = weight[2];
            edges[u].emplace_back(w, v);
            edges[v].emplace_back(u, w);
       }
        vector<int> minDist(n, INT_MAX); // 因为不会出现直接利用minDist数组中元素进行相加的情况所以可以直接初始化为INT_MAX
        minDist[k] = 0;
        // 使用优先队列（小顶堆）每次直接筛选出与源节点最近的节点
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        pq.emplace(0, k);

        while (!pq.empty()){
            auto p = pq.top();
            pq.pop();
            // p意味着当前循环中从源节点到to节点的距离最短为dis
            const auto &[dis, to] = p;
            if (dis > minDist[to]){
                continue;
            }
            // 利用to节点进行更新minDist
            for (const auto &edge : edges[to]){
                const auto &[d, nxt] = edge;
                int new_dis = d + dis;
                if (new_dis < minDist[nxt]){
                    minDist[nxt] = new_dis;
                    pq.emplace(new_dis, nxt);
                }
            }
        }

        return minDist;
    }
};
```
**复杂度分析**
- 时间复杂度：O(mlogm) m为边的个数。
- 空间复杂度：O(m)。