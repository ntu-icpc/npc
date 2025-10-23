---
title: C - Sodoku
layout: page
parent: NPC25 The 4th Contest Solution
---

# C - Sodoku

## Subtask 1
```cpp
#include <algorithm>
#include <cmath>
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

class Solution {
 public:
  /**
   * @param n: Number of nodes
   * @param m: Number of edges
   * @param corridors:a vector of vector of integers where each element is a
   * tuple (u, v, w) that represent a corridor between room u and room v with
   * speed requirement w (1-indexed)
   * @return: the speed required to run from room 1 to room n
   */
  static void dfs(int x, int pa, const vector<vector<pair<int, int>>>& e,
                  int& ans, int cur, int n) {
    if (x == n) {
      ans = cur;
      return;
    }
    for (auto [u, w] : e[x]) {
      if (u == pa) continue;
      dfs(u, x, e, ans, max(cur, w), n);
    }
  }
  static int solve(int n, int m, std::vector<std::vector<int>>& edges) {
    // Implement your solution by completing the following function
    int ans = 0;
    vector<vector<pair<int, int>>> e;
    e.resize(n + 1);
    for (auto x : edges) {
      e[x[0]].push_back(make_pair(x[1], x[2]));
      e[x[1]].push_back(make_pair(x[0], x[2]));
    }
    dfs(1, 1, e, ans, 0, n);
    return ans;
  }
};

int main() {
  int n, m;
  std::cin >> n >> m;
  std::vector<std::vector<int>> edges(m, std::vector<int>(3));
  for (int i = 0; i < m; ++i) {
    std::cin >> edges[i][0] >> edges[i][1] >> edges[i][2];
  }

  int ans = Solution::solve(n, m, edges);
  std::cout << ans << std::endl;

  return 0;
}
```
 
## Subtask 2
```cpp
#include <algorithm>
#include <cmath>
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

class Solution {
 public:
  /**
   * @param n: Number of nodes
   * @param m: Number of edges
   * @param corridors:a vector of vector of integers where each element is a
   * tuple (u, v, w) that represent a corridor between room u and room v with
   * speed requirement w (1-indexed)
   * @return: the speed required to run from room 1 to room n
   */
  static void dfs(int x, int pa, const vector<vector<pair<int, int>>>& e,
                  int& ans, int cur, int n, vector<int>& vis) {
    if (x == n) {
      ans = cur;
      return;
    }
    vis[x] = 1;
    for (auto [u, w] : e[x]) {
      if (u == pa || vis[u]) continue;
      dfs(u, x, e, ans, max(cur, w), n, vis);
    }
  }
  static int solve(int n, int m, std::vector<std::vector<int>>& edges) {
    // Implement your solution by completing the following function
    int ans = 0;
    for (int v = 1; v <= 1e9; v <<= 1) {
      vector<int> vis(n + 1, 0);
      vector<vector<pair<int, int>>> e;
      e.resize(n + 1);
      for (auto x : edges) {
        if (x[2] > v) continue;
        e[x[0]].push_back(make_pair(x[1], x[2]));
        e[x[1]].push_back(make_pair(x[0], x[2]));
      }
      ans = 0;
      dfs(1, 1, e, ans, 0, n, vis);
      if (ans) {
        return v;
      }
    }
    return ans;
  }
};

int main() {
  int n, m;
  std::cin >> n >> m;
  std::vector<std::vector<int>> edges(m, std::vector<int>(3));
  for (int i = 0; i < m; ++i) {
    std::cin >> edges[i][0] >> edges[i][1] >> edges[i][2];
  }

  int ans = Solution::solve(n, m, edges);
  std::cout << ans << std::endl;

  return 0;
}

```

## Solution (Minimum Spanning Tree)

{% tabs FourthC %}
{% tab FourthC Python %}
```python
from typing import List, Tuple

# If you want to use a deep recursion, you need to increase the recursion limit
# Python's default recursion limit is 1000
# Uncomment the following two lines if you need to use deep recursion
# import sys
# sys.setrecursionlimit(10**8)


class Solution:
    @staticmethod
    def solve(
        n: int,
        m: int,
        corridors: List[Tuple[int, int, int]],
    ) -> int:
        """
        Args:
            n: Number of rooms
            m: Number of corridors
            corridors: List of corridors as (u, v, w) tuple where u and v are 1-indexed node IDs.

        Returns:
            int: The minimum speed required to run from room 1 to room n
        """
        if n == 1:
            return 0

        parent = list(range(n + 1))

        def find(i: int) -> int:
            if parent[i] == i:
                return i
            parent[i] = find(parent[i])
            return parent[i]

        def union(i: int, j: int):
            root_i = find(i)
            root_j = find(j)
            if root_i != root_j:
                parent[root_i] = root_j

        corridors.sort(key=lambda x: x[2])

        for u, v, w in corridors:
            union(u, v)
            if find(1) == find(n):
                return w

        return -1


if __name__ == "__main__":
    n, m = map(int, input().split())
    corridors = []
    for _ in range(m):
        u, v, w = map(int, input().split())
        corridors.append((u, v, w))

    result = Solution.solve(n, m, corridors)
    print(result)
```
{% endtab %}
{% tab FourthC C %}
```c
/**
 * This template provides a complete implementation of a graph data structure
 * using a dynamic adjacency list representation. The implementation is designed
 * to be efficient for graph algorithms and problems.
 *
 * Adjacency List Implementation:
 * - Each node in the graph maintains its own list of neighbors (adjacent nodes)
 * - The neighbor list is implemented as a dynamic array that grows as needed
 * - Initial capacity is small (NEIGHBORS_INITIAL_CAP) and doubles when full
 * - This approach balances memory usage and performance
 *
 * Key Features:
 * - Dynamic memory allocation for efficient space usage
 * - O(1) edge insertion
 * - O(degree) time complexity for traversing neighbors of a node
 * - Support for undirected graphs
 *
 * Usage:
 * 1. Create a graph with get_graph(N) where N is the number of nodes
 * 2. Add edges with add_edge(graph, u, v) where u and v are node IDs
 * 3. Access neighbors of node i with graph->nodes[i].neighbors
 * 4. Number of neighbors is stored in graph->nodes[i].neighborsSize
 * 5. Remember to free the graph with free_graph() when done
 *
 * Note: All node IDs are 0-indexed in the internal representation
 * Input/output may use 1-indexed IDs, requiring conversion
 *
 */

#include <memory.h>
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>

#define NEIGHBORS_INITIAL_CAP 4  // Initial capacity for neighbors array

/**
 * @struct Guard
 * @brief Represents a guard with position and movement range
 */
typedef struct {
  int from;   // Node where corridor starts
  int to;     // Node where corridor ends
  int speed;  // speed requirement of the corridor
} Corridor;

/**
 * @struct Node
 * @brief Represents a node in the graph
 */
typedef struct {
  int id;               // Node identifier
  Corridor *neighbors;  // Dynamic array of connecting corridors
  int neighborsSize;    // Current number of neighbors
  int neighborsCap;     // Current capacity of neighbors array
} Node;

/**
 * @struct Graph
 * @brief Represents the entire graph
 */
typedef struct {
  Node *nodes;    // Array of all nodes
  int nodesSize;  // Number of nodes in the graph
} Graph;

/**
 * @brief Initialize a node with given ID
 * @param node Pointer to the node to initialize
 * @param id ID to assign to the node
 */
void init_node(Node *node, int id) {
  node->id = id;
  node->neighborsSize = 0;
  node->neighborsCap = NEIGHBORS_INITIAL_CAP;
  node->neighbors = calloc(node->neighborsCap, sizeof(Corridor));
  if (node->neighbors == NULL) {
    fprintf(stderr, "Memory allocation failed\n");
    exit(1);
  }
}

/**
 * @brief Free memory allocated for a node
 * @param node Pointer to the node to free
 */
void free_node(Node *node) { free(node->neighbors); }

/**
 * @brief Create a new graph with specified number of nodes
 * @param nodesSize Number of nodes in the graph
 * @return Pointer to the created graph
 */
Graph *get_graph(int nodesSize) {
  Graph *graph = malloc(sizeof(Graph));
  if (graph == NULL) {
    fprintf(stderr, "Memory allocation failed\n");
    exit(1);
  }
  graph->nodesSize = nodesSize;
  graph->nodes = calloc(nodesSize, sizeof(Node));
  if (graph->nodes == NULL) {
    fprintf(stderr, "Memory allocation failed\n");
    exit(1);
  }
  for (int i = 0; i < nodesSize; ++i) {
    init_node(&graph->nodes[i], i);
  }
  return graph;
}

/**
 * @brief Free memory allocated for a graph
 * @param graph Pointer to the graph to free
 */
void free_graph(Graph *graph) {
  for (int i = 0; i < graph->nodesSize; i++) {
    free_node(&graph->nodes[i]);
  }
  free(graph->nodes);
}

/**
 * @brief Add a neighbor to a node's neighbor list
 * @param node Pointer to the node
 * @param neighbor ID of the neighbor to add
 */
void insert_neighbor(Node *node, Corridor corridor) {
  // Double capacity if needed
  if (node->neighborsSize == node->neighborsCap) {
    node->neighborsCap *= 2;
    node->neighbors =
        realloc(node->neighbors, node->neighborsCap * sizeof(Corridor));
    if (node->neighbors == NULL) {
      fprintf(stderr, "Memory allocation failed\n");
      exit(1);
    }
  }

  node->neighbors[node->neighborsSize++] = corridor;
}

/**
 * @brief Add an undirected edge between two nodes in the graph
 * @param graph Pointer to the graph
 * @param u First node ID
 * @param v Second node ID
 */
void add_edge(Graph *graph, int u, int v, int speed) {
  if (u >= graph->nodesSize || v >= graph->nodesSize) {
    fprintf(stderr, "Invalid node id\n");
    exit(1);
  }
  Corridor c;
  c.from = u;
  c.to = v;
  c.speed = speed;
  insert_neighbor(&graph->nodes[u], c);
  c.from = v;
  c.to = u;
  insert_neighbor(&graph->nodes[v], c);
}

/**
 * @brief Comparison function for qsort to sort node IDs
 */
int cmp_result(const void *a, const void *b) { return *(int *)a - *(int *)b; }

// DSU find function with path compression
int find_set(int v, int *parent) {
  if (v == parent[v]) return v;
  return parent[v] = find_set(parent[v], parent);
}

// DSU union function
void unite_sets(int a, int b, int *parent) {
  a = find_set(a, parent);
  b = find_set(b, parent);
  if (a != b) parent[b] = a;
}

// Comparison function for qsort to sort Corridors by speed
int cmp_corridor(const void *a, const void *b) {
  Corridor *c1 = (Corridor *)a;
  Corridor *c2 = (Corridor *)b;
  if (c1->speed < c2->speed) return -1;
  if (c1->speed > c2->speed) return 1;
  return 0;
}

/**
 * @param n Number of room
 * @param m Number of corridors
 * @param graph Pointer to the graph (room are in 1-indexed)
 * @return The minimum speed required to run from room 1 to room n
 */
int solve(int n, int m, Graph *graph) {
  if (n == 1) {
    return 0;
  }
  if (m == 0) {
    return -1;
  }

  Corridor *corridors = malloc(m * sizeof(Corridor));
  if (corridors == NULL) {
    fprintf(stderr, "Memory allocation failed\n");
    exit(1);
  }
  int corridor_count = 0;
  for (int i = 1; i <= n; ++i) {
    Node node = graph->nodes[i];
    for (int j = 0; j < node.neighborsSize; ++j) {
      Corridor c = node.neighbors[j];
      if (c.from < c.to) {
        if (corridor_count < m) {
          corridors[corridor_count++] = c;
        }
      }
    }
  }

  qsort(corridors, m, sizeof(Corridor), cmp_corridor);

  int *parent = malloc((n + 1) * sizeof(int));
  if (parent == NULL) {
    fprintf(stderr, "Memory allocation failed\n");
    free(corridors);
    exit(1);
  }
  for (int i = 0; i <= n; i++) {
    parent[i] = i;
  }

  int result = -1;
  for (int i = 0; i < m; i++) {
    unite_sets(corridors[i].from, corridors[i].to, parent);
    if (find_set(1, parent) == find_set(n, parent)) {
      result = corridors[i].speed;
      break;
    }
  }

  free(parent);
  free(corridors);

  return result;
}

/**
 * @brief Main function to read input, solve the problem, and output results
 */
int main() {
  int n, m;
  scanf("%d%d", &n, &m);

  // Create graph with n nodes
  Graph *graph = get_graph(n + 1);

  // Read m edges
  for (int i = 0; i < m; ++i) {
    int u, v, w;
    scanf("%d%d%d", &u, &v, &w);
    add_edge(graph, u, v, w);
  }

  int result = solve(n, m, graph);

  printf("%d\n", result);

  // Clean up
  free_graph(graph);
  free(graph);
  return 0;
}
```
{% endtab %}
{% tab FourthC C++ %}
```cpp
#include <bits/stdc++.h>
using namespace std;

struct dsu {
  vector<int> parent;
  int n;
  dsu(int n) : n(n) {
    parent.resize(n);
    iota(parent.begin(), parent.end(), 0);
  }

  int find(int u) { return (parent[u] == u) ? u : parent[u] = find(parent[u]); }

  void merge(int u, int v) {
    u = find(u);
    v = find(v);
    if (u != v) parent[u] = v;
  }
};

int main() {
  int n, m;
  cin >> n >> m;
  vector<tuple<int, int, int>> edges(m);
  for (int i = 0; i < m; i++) {
    int u, v, w;
    cin >> u >> v >> w;
    u--, v--;
    edges[i] = {w, u, v};
  }
  sort(edges.begin(), edges.end());
  int ans = 0;
  dsu d(n);
  for (auto& [w, u, v] : edges) {
    d.merge(u, v);
    if (d.find(0) == d.find(n - 1)) {
      ans = w;
      break;
    }
  }
  cout << ans << '\n';
}

```
{% endtab %}
{% tab FourthC Java %}
```java
import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;
import java.util.Scanner;

// even though the data is from and to. But it is an undirected edge. The From and To is just for
// adjacentcy listpurpose
class Corridor {
  int from;
  int to;
  int w;

  Corridor(int from, int to, int w) {
    this.from = from;
    this.to = to;
    this.w = w;
  }
}

class Graph {
  List<List<Corridor>> adj;

  Graph(int n) {
    adj = new ArrayList<>();
    for (int i = 0; i <= n; i++) {
      adj.add(new ArrayList<>());
    }
  }

  public void addEdge(int u, int v, int w) {
    adj.get(u).add(new Corridor(u, v, w));
    adj.get(v).add(new Corridor(v, u, w));
  }

  public List<Corridor> getListOfAdjecentNodes(int u) {
    return adj.get(u);
  }
}

class Solution {
  static class DSU {
    int[] parent;

    DSU(int n) {
      parent = new int[n + 1];
      for (int i = 0; i <= n; i++) {
        parent[i] = i;
      }
    }

    int find(int x) {
      if (parent[x] != x) {
        parent[x] = find(parent[x]);
      }
      return parent[x];
    }

    void union(int x, int y) {
      int rootX = find(x);
      int rootY = find(y);
      if (rootX != rootY) {
        parent[rootX] = rootY;
      }
    }
  }

  /**
   * @param n: number of rooms
   * @param m: number of corridors
   * @param corridors: list of Corridor objects (u, v, w) representing a corridor between room u and
   *     room v with speed requirement w, where u and v are 1-indexed node IDs.
   * @return: return an integer representing the minimum speed required to run from room 1 to room n
   */
  static int solve(int n, int m, List<Corridor> corridors) {
    if (n == 1) {
      return 0;
    }

    corridors.sort(Comparator.comparingInt(c -> c.w));

    DSU dsu = new DSU(n);

    for (Corridor corridor : corridors) {
      dsu.union(corridor.from, corridor.to);
      if (dsu.find(1) == dsu.find(n)) {
        return corridor.w;
      }
    }

    return -1;
  }

  // ALTERNATIVE
  /**
   * @param n: number of rooms
   * @param m: number of corridors
   * @param maze: an adjacency list representing the maze, where maze[i] contains list of room
   *     adjacent to it and the speed requirement of the corridor connecting them.
   * @return: return an integer representing the minimum speed required to run from room 1 to room n
   */
  static int solve(int n, int m, Graph maze) {
    // Implement your solution here
    return 0;
  }

  public static void main(String[] args) throws java.lang.Exception {
    Scanner input = new Scanner(System.in);

    int n = input.nextInt();
    int m = input.nextInt();

    List<Corridor> corridors = new ArrayList<>();
    Graph maze = new Graph(n + 1);
    for (int i = 0; i < m; i++) {
      int u, v, w;
      u = input.nextInt();
      v = input.nextInt();
      w = input.nextInt();
      corridors.add(new Corridor(u, v, w));
      maze.addEdge(u, v, w);
    }

    int ans1 = solve(n, m, corridors);
    int ans2 = solve(n, m, maze);
    int ans = ans1;
    if (ans1 == 0 && n > 1)
      ans = ans2;
    System.out.println(ans);

    input.close();
  }
}

```
{% endtab %}
{% tab FourthC Rust %}
```rust
use std::io;

fn find(x: usize, fa: &mut [i32]) -> i32 {
    if fa[x] != x as i32 {
        fa[x] = find(fa[x] as usize, fa);
    }
    fa[x]
}

fn main() {
    let mut input = String::new();
    io::stdin().read_line(&mut input).expect("Failed to read line");
    let nums: Vec<usize> = input
        .trim()
        .split_whitespace()
        .map(|s| s.parse().expect(""))
        .collect();
    let (n, m) = (nums[0], nums[1]);

    let mut edges = Vec::new();
    for _ in 0..m {
        input.clear();
        io::stdin().read_line(&mut input).expect("Failed to read line");
        let edge: Vec<usize> = input
            .trim()
            .split_whitespace()
            .map(|s| s.parse().expect(""))
            .collect();
        let (u, v, w) = (edge[0], edge[1], edge[2]);
        edges.push((u, v, w));
    }
    edges.sort_by_key(|x| x.2);
    let mut fa: Vec<i32> = (0..=n as i32).collect();
    for i in 0..m {
        let (u, v, w) = edges[i];
        let fu = find(u, &mut fa);
        let fv = find(v, &mut fa);
        if fu != fv {
            fa[fu as usize] = fv;
        }
        if find(1, &mut fa) == find(n, &mut fa) {
            println!("{}", w);
            return;
        }
    }
    println!("-1");
}

```
{% endtab %}
{% endtabs %}

## Solution (Binary Search On Answer)
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  int n, m;
  cin >> n >> m;
  vector<vector<pair<int, int> > > adj(n);

  for (int i = 0; i < m; i++) {
    int u, v, w;
    cin >> u >> v >> w;
    u--, v--;
    adj[u].emplace_back(v, w);
    adj[v].emplace_back(u, w);
  }

  vector<int> visited(n);
  int limit;
  function<void(int)> dfs = [&](int u) {
    visited[u] = 1;
    for (auto& [v, w] : adj[u]) {
      if (!visited[v] && w <= limit) dfs(v);
    }
  };

  int l = 1, r = 1e9;
  while (l < r) {
    int mid = (l + r) / 2;

    visited.assign(n, 0);
    limit = mid;
    dfs(0);
    if (visited[n - 1])
      r = mid;
    else
      l = mid + 1;
  }

  cout << l << "\n";
}
```