[参考灵神写法](https://leetcode.cn/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/solutions/2525946/dai-ni-fa-ming-floyd-suan-fa-cong-ji-yi-m8s51/)

## 定义

中间节点（不包含起点和终点）为最短路上不是终点和起点的节点

## 思路

从编号最大的节点开始思考，选与不选。

dfs(k, i, j)代表中间节点 <= k，从i至j的最短路径长度。

- 不选，那么中间节点只能 <= k - 1，dfs(k, i, j) = dfs(k - 1, i, j);
- 选，问题转化为 i-->k + k -->j的最短路径即 dfs(k, i, j) = dfs(k - 1, i, k) + dfs(k - 1, k, j);

初始化k = -1，此时无中间节点，起点和终点直接相连。

## 实现

直接写空间优化的递推

```cpp
    void Floyd(vector<vector<int>> &f){
		int n = f.size();
        
        for (int k = 0; k < n; ++k){
            for (int i = 0; i < n; ++i){
                if (f[i][k] == INT_MAX / 2){
                    continue;
                }
                for (int j = 0; j < n; ++j){
                    f[i][j] = min(f[i][j], f[i][k] + f[k][j]);
                }
            }
        }
    }
```

