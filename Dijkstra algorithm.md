#include <iostream>
#include <vector>
#include <queue>
#include <utility>
#include <climits>

using namespace std;

// 图的邻接表结构：每个点 i 对应一个 vector 存储 (v, w)
using Edge = pair<int, int>; // {邻接点编号, 边权}
using Graph = vector<vector<Edge>>;

vector<int> dijkstra(const Graph& adj, int start) {
    int n = adj.size();  // 节点数
    vector<int> dist(n, INT_MAX); // 最短距离数组，初始为正无穷
    dist[start] = 0;

    // 小根堆：按照距离排序 (dist, node)
    priority_queue<Edge, vector<Edge>, greater<Edge>> pq;
    pq.emplace(0, start);

    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();

        // 如果弹出的已经不是最短路径（有更短的提前更新过），跳过
        if (d > dist[u]) continue;

        for (auto [v, w] : adj[u]) {
            if (dist[v] > dist[u] + w) {
                dist[v] = dist[u] + w;
                pq.emplace(dist[v], v);
            }
        }
    }

    return dist;
}
