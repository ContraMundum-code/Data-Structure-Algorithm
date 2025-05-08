

🧠 Floyd-Warshall Algorithm Overview

✅ Purpose
	•	Compute the shortest distances between all pairs of vertices in a weighted graph.

⚙️ Key Idea: Dynamic Programming
	•	Gradually allow more intermediate vertices (from 0 to V−1) on the path between every pair of nodes.
	•	Transition formula:

dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])



⸻

🔍 Assumptions & Requirements
	•	Supports negative edge weights ✅
	•	Detects negative weight cycles ✅
	•	No parallel edges — assume graph is reduced
	•	Graph is given as an adjacency matrix or edge list

⸻

⏱️ Complexity

Metric	Value
Time Complexity	O(V³)
Space Complexity	O(V²)
Best Use Case	Small graphs (V ≤ 500) with many edges (dense graphs)



⸻

🧮 Pseudocode / C++ Template

```cpp
void floydWarshall(vector<vector<int>>& dist) {
    int V = dist.size();
    for (int k = 0; k < V; ++k)          // Intermediate node
        for (int i = 0; i < V; ++i)      // Start node
            for (int j = 0; j < V; ++j)  // End node
                if (dist[i][k] != INT_MAX && dist[k][j] != INT_MAX)
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
}
INT_MAX indicates no edge between i and j (∞).

⸻

⚠️ Detecting Negative Weight Cycles

After the algorithm:

for (int i = 0; i < V; ++i)
    if (dist[i][i] < 0)
        cout << "Graph contains a negative weight cycle\n";



⸻

🔁 Path Reconstruction (Optional)

Use a next[i][j] matrix to store intermediate path information.

Initialization:

if (i != j && dist[i][j] != INF)
    next[i][j] = j;

Update in Floyd:

if (dist[i][j] > dist[i][k] + dist[k][j]) {
    dist[i][j] = dist[i][k] + dist[k][j];
    next[i][j] = next[i][k];
}

Path recovery:

vector<int> getPath(int u, int v) {
    if (next[u][v] == -1) return {};
    vector<int> path = {u};
    while (u != v) {
        u = next[u][v];
        path.push_back(u);
    }
    return path;
}



⸻

🧪 Example Input Matrix

// Adjacency matrix, INT_MAX = ∞
vector<vector<int>> dist = {
    {0,     3,   INT_MAX, 5},
    {2,     0,   INT_MAX, 4},
    {INT_MAX, 1, 0,     INT_MAX},
    {INT_MAX, INT_MAX, 2, 0}
};



⸻

✅ When to Use Floyd-Warshall

Situation	Use Floyd?
Graph has ≤ 500 nodes	✅ Yes
You need all-pairs shortest paths	✅ Yes
You need to detect negative cycles	✅ Yes
Graph is sparse	❌ No (Dijkstra or Johnson’s better)
Only need single-source shortest path	❌ Use Dijkstra / Bellman-Ford

