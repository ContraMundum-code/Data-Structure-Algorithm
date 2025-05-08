

🧠 Bellman-Ford 算法

✅ 适用场景
	•	单源最短路径
	•	图中存在负权边
	•	可以检测负权环

🔧 算法核心思想

对所有边进行“松弛操作”：

如果 dist[u] + w < dist[v]，就更新 dist[v] = dist[u] + w

重复执行 V−1 轮，每轮对所有边执行一次松弛。

🧮 时间复杂度
	•	最坏时间复杂度：O(V × E)
	•	空间复杂度：O(V)

📦 标准实现（C++）

bool bellmanFord(int V, vector<tuple<int, int, int>> &edges, int start, vector<int>& dist) {
    dist.assign(V, INT_MAX);
    dist[start] = 0;

    for (int i = 1; i < V; ++i) {
        for (auto [u, v, w] : edges) {
            if (dist[u] != INT_MAX && dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
            }
        }
    }

    // 检测负权环
    for (auto [u, v, w] : edges) {
        if (dist[u] != INT_MAX && dist[u] + w < dist[v])
            return false; // 存在负环
    }
    return true;
}



⸻

🧠 SPFA（Shortest Path Faster Algorithm）

✅ 适用场景
	•	单源最短路径
	•	存在负权边
	•	实际运行快于 Bellman-Ford（尤其在稀疏图中）
	•	可以检测负权环

🔧 核心思想
	•	用队列优化：只处理可能导致更新的点
	•	如果某个点的最短路径值更新了，把它的邻居重新加入队列进行松弛

🧮 时间复杂度
	•	最坏：O(VE)
	•	平均：远优于 Bellman-Ford

📦 标准实现（C++）

bool spfa(const vector<vector<pair<int, int>>>& adj, int start, vector<int>& dist) {
    int V = adj.size();
    dist.assign(V, INT_MAX);
    vector<bool> inQueue(V, false);
    vector<int> count(V, 0); // 入队次数

    queue<int> q;
    dist[start] = 0;
    q.push(start);
    inQueue[start] = true;
    count[start]++;

    while (!q.empty()) {
        int u = q.front(); q.pop();
        inQueue[u] = false;

        for (auto [v, w] : adj[u]) {
            if (dist[u] != INT_MAX && dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                if (!inQueue[v]) {
                    q.push(v);
                    inQueue[v] = true;
                    count[v]++;
                    if (count[v] >= V) return false; // 检测负环
                }
            }
        }
    }

    return true;
}



⸻

📊 Bellman-Ford vs SPFA 对比表

特性	Bellman-Ford	SPFA
支持负边权	✅ 是	✅ 是
支持负权环检测	✅ 是	✅ 是
时间复杂度	O(VE)	最坏 O(VE)，平均远快
编码难度	简单	略复杂，需要队列与标记
是否适用于稀疏图	一般	✅ 非常适合
是否工程常用	一般	✅ 非常常用



⸻

✏️ 适用建议
	•	如果只要求正确，图较小或理解为主 ➜ 用 Bellman-Ford
	•	如果图较稀疏、节点多、希望效率高 ➜ 用 SPFA
	•	如果怀疑图中有负环，两者都可检测

