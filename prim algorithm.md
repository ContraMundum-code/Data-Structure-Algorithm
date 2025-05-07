# Prim 算法（最小生成树）

## 📌 算法简介
Prim 是一种贪心算法，用于在加权无向图中寻找最小生成树...

## 🧠 思路概述
从一个起点开始，每次选择一条连接“树内”和“树外”的最小边...

## ⏱️ 时间复杂度
O(E log V)，使用小根堆优化

## ✅ 示例代码（C++）

```cpp
// C++ 实现见下方
# Prim 算法模板（C++）

适用于稠密图，用邻接表 + 小根堆优化。

## 🌟 模板代码

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>

using namespace std;

using Edge = pair<int, int>; // {边权重, 目标节点}
using Graph = vector<vector<Edge>>;

int prim(const Graph& adj, int start = 0) {
    int n = adj.size();
    vector<bool> visited(n, false);
    priority_queue<Edge, vector<Edge>, greater<Edge>> pq;
    int total_weight = 0;

    pq.emplace(0, start);

    while (!pq.empty()) {
        auto [w, u] = pq.top(); pq.pop();
        if (visited[u]) continue;
        visited[u] = true;
        total_weight += w;

        for (auto [vw, v] : adj[u]) {
            if (!visited[v]) pq.emplace(vw, v);
        }
    }

    return total_weight;
}
