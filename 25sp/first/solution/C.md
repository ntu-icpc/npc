---
title: C - Graph
layout: page
parent: Solution
---

# C - Graph

## Codes

### C

```c
#include <memory.h>
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>

#define NEIGHBORS_INITIAL_CAP 4  // Initial capacity for neighbors array

/**
 * @struct Node
 * @brief Represents a node in the graph
 */
typedef struct {
  int id;             // Node identifier
  int *neighbors;     // Dynamic array of neighboring node IDs
  int neighborsSize;  // Current number of neighbors
  int neighborsCap;   // Current capacity of neighbors array
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
 * @struct Guard
 * @brief Represents a guard with position and movement range
 */
typedef struct {
  int node;  // Node where the guard is located
  int h;     // Movement range of the guard
} Guard;

/**
 * @struct Result
 * @brief Stores the result of the problem
 */
typedef struct {
  int G;   // Number of nodes that can be protected
  int *v;  // Array of protected node IDs
} Result;

/**
 * @brief Initialize a node with given ID
 * @param node Pointer to the node to initialize
 * @param id ID to assign to the node
 */
void init_node(Node *node, int id) {
  node->id = id;
  node->neighborsSize = 0;
  node->neighborsCap = NEIGHBORS_INITIAL_CAP;
  node->neighbors = calloc(node->neighborsCap, sizeof(int));
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
void insert_neighbor(Node *node, int neighbor) {
  // Double capacity if needed
  if (node->neighborsSize == node->neighborsCap) {
    node->neighborsCap *= 2;
    node->neighbors =
        realloc(node->neighbors, node->neighborsCap * sizeof(int));
    if (node->neighbors == NULL) {
      fprintf(stderr, "Memory allocation failed\n");
      exit(1);
    }
  }
  node->neighbors[node->neighborsSize++] = neighbor;
}

/**
 * @brief Add an undirected edge between two nodes in the graph
 * @param graph Pointer to the graph
 * @param u First node ID
 * @param v Second node ID
 */
void add_edge(Graph *graph, int u, int v) {
  if (u >= graph->nodesSize || v >= graph->nodesSize) {
    fprintf(stderr, "Invalid node id\n");
    exit(1);
  }
  insert_neighbor(&graph->nodes[u], v);
  insert_neighbor(&graph->nodes[v], u);
}

/**
 * @brief Comparison function for qsort to sort node IDs
 */
int cmp_result(const void *a, const void *b) { return *(int *)a - *(int *)b; }

/**
 * @struct Heap
 * @brief Max-heap implementation for priority queue of guards
 */
typedef struct {
  Guard *data;  // Array of guards
  int size;     // Current number of elements
  int cap;      // Current capacity
} Heap;

/**
 * @brief Get the index of the left child in the heap
 * @param x Index of the parent
 * @return Index of the left child
 */
int heap_left_son(int x) { return 2 * x + 1; }

/**
 * @brief Get the index of the right child in the heap
 * @param x Index of the parent
 * @return Index of the right child
 */
int heap_right_son(int x) { return 2 * x + 2; }

/**
 * @brief Get the index of the parent in the heap
 * @param x Index of the child
 * @return Index of the parent
 */
int heap_parent(int x) { return (x - 1) / 2; }

/**
 * @brief Swap two guards in the heap
 * @param a Pointer to first guard
 * @param b Pointer to second guard
 */
void swap(Guard *a, Guard *b) {
  Guard tmp = *a;
  *a = *b;
  *b = tmp;
}

/**
 * @brief Move an element up the heap to maintain heap property
 * @param heap Pointer to the heap
 * @param x Index of the element to sift up
 */
void sift_up(Heap *heap, int x) {
  while (x > 0) {
    int parent = heap_parent(x);
    if (heap->data[x].h > heap->data[parent].h) {
      swap(&heap->data[x], &heap->data[parent]);
      x = parent;
    } else {
      break;
    }
  }
}

/**
 * @brief Move an element down the heap to maintain heap property
 * @param heap Pointer to the heap
 * @param x Index of the element to sift down
 */
void sift_down(Heap *heap, int x) {
  int left = heap_left_son(x);
  int right = heap_right_son(x);

  int largest = x;
  // Find the largest among the node and its children
  if (left < heap->size && heap->data[left].h > heap->data[largest].h) {
    largest = left;
  }
  if (right < heap->size && heap->data[right].h > heap->data[largest].h) {
    largest = right;
  }

  // If largest is not the current node, swap and continue sifting down
  if (largest != x) {
    swap(&heap->data[x], &heap->data[largest]);
    sift_down(heap, largest);
  }
}

/**
 * @brief Convert an array into a heap
 * @param heap Pointer to the heap
 */
void heapify(Heap *heap) {
  // Start from the last non-leaf node and sift down each node
  for (int i = heap->size / 2 - 1; i >= 0; --i) {
    sift_down(heap, i);
  }
}

/**
 * @brief Add a guard to the heap
 * @param heap Pointer to the heap
 * @param guard Guard to add
 */
void heap_push(Heap *heap, Guard guard) {
  // Double capacity if needed
  if (heap->size == heap->cap) {
    heap->cap *= 2;
    heap->data = realloc(heap->data, heap->cap * sizeof(Guard));
    if (heap->data == NULL) {
      fprintf(stderr, "Memory allocation failed\n");
      exit(1);
    }
  }
  heap->data[heap->size++] = guard;
  sift_up(heap, heap->size - 1);
}

/**
 * @brief Remove and return the guard with highest h value
 * @param heap Pointer to the heap
 * @return Guard with highest h value
 */
Guard heap_pop(Heap *heap) {
  Guard result = heap->data[0];
  heap->data[0] = heap->data[--heap->size];
  sift_down(heap, 0);
  return result;
}

/**
 * @brief Create a new heap from an array of guards
 * @param data Array of guards
 * @param k Number of guards
 * @return Pointer to the created heap
 */
Heap *heap_build(Guard *data, int k) {
  Heap *heap = malloc(sizeof(Heap));
  if (heap == NULL) {
    fprintf(stderr, "Memory allocation failed\n");
    exit(1);
  }
  heap->data = calloc(k, sizeof(Guard));
  if (heap->data == NULL) {
    fprintf(stderr, "Memory allocation failed\n");
    exit(1);
  }
  memcpy(heap->data, data, k * sizeof(Guard));
  heap->size = k;
  heap->cap = k;
  heapify(heap);
  return heap;
}

/**
 * @brief Free memory allocated for a heap
 * @param heap Pointer to the heap to free
 */
void heap_free(Heap *heap) { free(heap->data); }

/**
 * @brief Check if the heap is empty
 * @param heap Pointer to the heap
 * @return true if empty, false otherwise
 */
bool heap_empty(Heap *heap) { return heap->size == 0; }

/**
 * @brief Solve the problem using a modified Dijkstra's algorithm
 * @param n Number of nodes
 * @param m Number of edges
 * @param k Number of guards
 * @param graph Pointer to the graph
 * @param guards Array of guards
 * @return Result containing protected nodes
 */
Result solve(int n, int m, int k, Graph *graph, Guard *guards) {
  // Create a max-heap of guards prioritized by movement range
  Heap *heap = heap_build(guards, k);

  // Track visited nodes
  bool *visited = calloc(n, sizeof(bool));
  memset(visited, 0, n * sizeof(bool));

  // Process guards in order of decreasing movement range
  while (!heap_empty(heap)) {
    Guard guard = heap_pop(heap);
    if (visited[guard.node]) {
      continue;  // Skip if node already visited
    }
    visited[guard.node] = true;
    if (guard.h == 0) {
      continue;  // Guard can't move further
    }

    // Explore neighbors with reduced movement range
    for (int i = 0; i < graph->nodes[guard.node].neighborsSize; ++i) {
      int neighbor = graph->nodes[guard.node].neighbors[i];
      if (!visited[neighbor]) {
        heap_push(heap, (Guard){neighbor, guard.h - 1});
      }
    }
  }

  heap_free(heap);

  // Collect results
  Result result;
  result.G = 0;
  // Count protected nodes
  for (int i = 0; i < n; ++i) {
    if (visited[i]) {
      result.G++;
    }
  }

  // Store protected node IDs
  result.v = calloc(result.G, sizeof(int));
  for (int i = 0, j = 0; i < n; ++i) {
    if (visited[i]) {
      result.v[j++] = i;
    }
  }

  free(visited);
  return result;
}

/**
 * @brief Main function to read input, solve the problem, and output results
 */
int main() {
  int n, m, k;
  scanf("%d%d%d", &n, &m, &k);

  // Create graph with n nodes
  Graph *graph = get_graph(n);

  // Read m edges
  for (int i = 0; i < m; ++i) {
    int u, v;
    scanf("%d%d", &u, &v);
    add_edge(graph, u - 1, v - 1);  // Convert to 0-indexed
  }

  // Read k guards
  Guard *guards = calloc(k, sizeof(Guard));
  if (guards == NULL) {
    fprintf(stderr, "Memory allocation failed\n");
    exit(1);
  }
  for (int i = 0; i < k; ++i) {
    scanf("%d%d", &guards[i].node, &guards[i].h);
    guards[i].node--;  // Convert to 0-indexed
  }

  // Solve the problem
  Result result = solve(n, m, k, graph, guards);

  // Sort the result for consistent output
  qsort(result.v, result.G, sizeof(int), cmp_result);

  // Print results (convert back to 1-indexed)
  printf("%d\n", result.G);
  for (int i = 0; i < result.G; ++i) {
    printf("%d ", result.v[i] + 1);
  }
  printf("\n");

  // Clean up
  free(result.v);
  free(guards);
  free_graph(graph);
  free(graph);
  return 0;
}
```

### C++

```cpp
#include <algorithm>
#include <iostream>
#include <queue>
#include <vector>

struct Guard {
  int pos;
  int power;
  Guard(int p, int pow) : pos(p), power(pow) {}

  int getPos() const { return pos; }
  int getPower() const { return power; }
};

struct GuardComparator {
  bool operator()(const Guard& g1, const Guard& g2) {
    return g1.getPower() <
           g2.getPower();  // Min-heap by default, so we reverse it
  }
};

class Solution {
 public:
  /**
   * @param n: Number of nodes
   * @param m: Number of edges
   * @param k: Number of security guards
   * @param edges: a vector of vector of integers where each element is a pair
   * that represent an edge
   * @param guards: a vector of vector of integers where each element is a pair
   * representing the position and guarding power
   * @return: return the list of nodes that are covered by guards
   */
  static std::vector<int> solve(int n, int m, int k,
                                std::vector<std::vector<int>>& edges,
                                std::vector<std::vector<int>>& guards) {
    std::vector<std::vector<int>> adj(n + 1);
    for (int i = 0; i < m; ++i) {
      adj[edges[i][0]].push_back(edges[i][1]);
      adj[edges[i][1]].push_back(edges[i][0]);
    }
    std::priority_queue<Guard, std::vector<Guard>, GuardComparator> pq;
    std::vector<bool> covered(n + 1, false);
    for (int i = 0; i < k; ++i) {
      pq.push(Guard(guards[i][0], guards[i][1]));
    }
    while (!pq.empty()) {
      Guard g = pq.top();
      pq.pop();
      int power = g.getPower();
      int pos = g.getPos();
      if (covered[pos]) continue;
      covered[pos] = true;
      if (power == 0) continue;
      for (int next : adj[pos]) {
        if (!covered[next]) {
          pq.push(Guard(next, power - 1));
        }
      }
    }
    std::vector<int> ans;
    for (int i = 1; i <= n; ++i) {
      if (covered[i]) {
        ans.push_back(i);
      }
    }
    return ans;
  }
};

int main() {
  int n, m, k;
  std::cin >> n >> m >> k;
  std::vector<std::vector<int>> edges(m, std::vector<int>(2));
  for (int i = 0; i < m; ++i) {
    std::cin >> edges[i][0] >> edges[i][1];
  }
  std::vector<std::vector<int>> guards(k, std::vector<int>(2));
  for (int i = 0; i < k; ++i) {
    std::cin >> guards[i][0] >> guards[i][1];
  }
  std::vector<int> ans = Solution::solve(n, m, k, edges, guards);
  std::cout << ans.size() << std::endl;
  for (int node : ans) {
    std::cout << node << " ";
  }
  std::cout << std::endl;
  return 0;
}
```

### Java

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.PriorityQueue;
import java.util.Scanner;

class Guard {
  int pos;
  int power;
  Guard(int pos, int power) {
    this.pos = pos;
    this.power = power;
  }

  public int getPos() {
    return this.pos;
  }

  public int getPower() {
    return this.power;
  }
}

class Graph {
  List<List<Integer>> adj;

  Graph(int n) {
    adj = new ArrayList<>();
    for (int i = 0; i <= n; i++) {
      adj.add(new ArrayList<>());
    }
  }

  public void addEdge(int u, int v) {
    adj.get(u).add(v);
    adj.get(v).add(u);
  }

  public List<Integer> getListOfAdjecentNodes(int u) {
    return adj.get(u);
  }
}

class Main {
  /**
   * @param n: Number of nodes
   * @param m: Number of edges
   * @param k: number of security guards
   * @param edges: a list of list of integers where each element is a list of two integers that
   *     represent an edge
   * @param guards: a list of list of integers where each element is a list of two integers
   *     represent the position and guarding power
   * @return: return the maximum achieveable fullness
   */
  public static List<Integer> solve(int n, int m, int k, int edges[][], int guards[][]) {
    // Implement your solution here

    List<List<Integer>> adj = new ArrayList<>();
    for (int i = 0; i <= n; i++) {
      adj.add(new ArrayList<>());
    }
    for (int i = 0; i < m; i++) {
      adj.get(edges[i][0]).add(edges[i][1]);
      adj.get(edges[i][1]).add(edges[i][0]);
    }

    PriorityQueue<Guard> pq = new PriorityQueue<>(Comparator.comparing(Guard::getPower).reversed());
    boolean[] covered = new boolean[n + 1];
    Arrays.fill(covered, false);
    for (int i = 0; i < k; i++) {
      pq.add(new Guard(guards[i][0], guards[i][1]));
    }

    while (!pq.isEmpty()) {
      Guard g = pq.poll();
      int power = g.getPower();
      int pos = g.getPos();
      if (covered[pos])
        continue;
      covered[pos] = true;
      if (power == 0)
        continue;
      for (int i = 0; i < adj.get(pos).size(); i++) {
        int next = adj.get(pos).get(i);
        if (!covered[next]) {
          pq.add(new Guard(next, power - 1));
        }
      }
    }

    List<Integer> ans = new ArrayList<>();
    for (int i = 1; i <= n; i++) {
      if (covered[i])
        ans.add(i);
    }
    return ans;
  }

  // ALTERNATIVE
  /**
   * @param n: Number of nodes
   * @param m: Number of edges
   * @param k: number of security guards
   * @param graphs: an object of the class Graph
   * @param guards: a list of object of the class Guard
   * @return: return the list of nodes that are covered by the guards
   */
  // public static List<Integer> solve(int n, int m, int k, int edges[][], int guards[][]) {
  // }
  public static List<Integer> solve(int n, int m, int k, Graph graph, Guard[] guards) {
    // Implement your solution here

    PriorityQueue<Guard> pq = new PriorityQueue<>(Comparator.comparing(Guard::getPower).reversed());
    boolean[] covered = new boolean[n + 1];
    Arrays.fill(covered, false);
    for (int i = 0; i < k; i++) {
      pq.add(guards[i]);
    }

    while (!pq.isEmpty()) {
      Guard g = pq.poll();
      int power = g.getPower();
      int pos = g.getPos();
      if (covered[pos])
        continue;
      covered[pos] = true;
      if (power == 0)
        continue;
      List<Integer> adjacentNode = graph.getListOfAdjecentNodes(pos);
      for (int i = 0; i < adjacentNode.size(); i++) {
        int next = adjacentNode.get(i);
        if (!covered[next]) {
          pq.add(new Guard(next, power - 1));
        }
      }
    }

    List<Integer> ans = new ArrayList<>();
    for (int i = 1; i <= n; i++) {
      if (covered[i])
        ans.add(i);
    }
    return ans;
  }

  public static void main(String[] args) throws java.lang.Exception {
    Scanner input = new Scanner(System.in);

    int n = input.nextInt();
    int m = input.nextInt();
    int k = input.nextInt();
    int edges[][] = new int[m][2];
    Graph graph = new Graph(n);

    for (int i = 0; i < m; i++) {
      edges[i][0] = input.nextInt();
      edges[i][1] = input.nextInt();
      graph.addEdge(edges[i][0], edges[i][1]);
    }

    Guard[] guards = new Guard[k];
    int guardsInArray[][] = new int[k][2];

    for (int i = 0; i < k; i++) {
      guardsInArray[i][0] = input.nextInt();
      guardsInArray[i][1] = input.nextInt();
      guards[i] = new Guard(guardsInArray[i][0], guardsInArray[i][1]);
    }

    List<Integer> ans1 = solve(n, m, k, edges, guardsInArray);
    List<Integer> ans2 = solve(n, m, k, graph, guards);
    List<Integer> ans = new ArrayList<>();

    if (ans1.size() != 0)
      ans = ans1;
    if (ans2.size() != 0)
      ans = ans2;

    System.out.println(ans.size());
    for (int i = 0; i < ans.size(); i++) {
      System.out.print(ans.get(i) + " ");
    }
    System.out.println();
    input.close();
  }
}
```

### Python

```python
from dataclasses import dataclass, field


@dataclass(order=True)
class GuardedNode:
    """
    A dataclass representing a node with a guard.

    Attributes:
        node (int): The index of the node in the graph (0-indexed internally).
        h (int): The strength of the guard at this node (how far it can protect).

    Note:
        The comparison is based only on the guard strength (h) for heap operations.
    """

    node: int = field(compare=False)
    h: int


class Heap:
    """
    A max-heap implementation specialized for GuardedNode objects.

    This heap prioritizes nodes with higher guard strength values.
    """

    def __init__(self, guarded_nodes: list[GuardedNode]):
        """
        Initialize a heap with a list of GuardedNode objects.

        Args:
            guarded_nodes: List of GuardedNode objects to initialize the heap.
        """
        self.heap = guarded_nodes
        for i in range(self.n // 2 - 1, -1, -1):
            self.sift_down(i)

    def __len__(self):
        """Return the number of elements in the heap."""
        return len(self.heap)

    @property
    def n(self):
        """Property that returns the size of the heap."""
        return len(self)

    def push(self, node: GuardedNode):
        """
        Add a new GuardedNode to the heap.

        Args:
            node: The GuardedNode to add to the heap.
        """
        self.heap.append(node)
        self.sift_up(self.n - 1)

    def empty(self):
        """
        Check if the heap is empty.

        Returns:
            bool: True if the heap is empty, False otherwise.
        """
        return self.n == 0

    def pop(self) -> GuardedNode:
        """
        Remove and return the GuardedNode with the highest guard strength.

        Returns:
            GuardedNode: The node with the highest guard strength.

        Raises:
            IndexError: If the heap is empty.
        """
        if self.empty():
            raise IndexError("pop from an empty heap")

        self.heap[0], self.heap[-1] = self.heap[-1], self.heap[0]
        node = self.heap.pop()

        if not self.empty():
            self.sift_down(0)

        return node

    def sift_up(self, x: int):
        """
        Move a node up the heap to maintain the heap property.

        Args:
            x: The index of the node to sift up.
        """
        while x > 0:
            parent = Heap.parent(x)
            if self.heap[x].h > self.heap[parent].h:
                self.heap[x], self.heap[parent] = self.heap[parent], self.heap[x]
                x = parent
            else:
                break

    def sift_down(self, x: int):
        """
        Move a node down the heap to maintain the heap property.

        Args:
            x: The index of the node to sift down.
        """
        left = Heap.left_son(x)
        right = Heap.right_son(x)

        largest = x
        if left < self.n and self.heap[left].h > self.heap[largest].h:
            largest = left
        if right < self.n and self.heap[right].h > self.heap[largest].h:
            largest = right

        if largest != x:
            self.heap[x], self.heap[largest] = self.heap[largest], self.heap[x]
            self.sift_down(largest)

    @staticmethod
    def left_son(x: int):
        """
        Calculate the index of the left child of a node.

        Args:
            x: The index of the parent node.

        Returns:
            int: The index of the left child.
        """
        return 2 * x + 1

    @staticmethod
    def right_son(x: int):
        """
        Calculate the index of the right child of a node.

        Args:
            x: The index of the parent node.

        Returns:
            int: The index of the right child.
        """
        return 2 * x + 2

    @staticmethod
    def parent(x: int):
        """
        Calculate the index of the parent of a node.

        Args:
            x: The index of the child node.

        Returns:
            int: The index of the parent.
        """
        return (x - 1) // 2


class Solution:
    @staticmethod
    def solve(
        n: int,
        m: int,
        k: int,
        edges: list[tuple[int, int]],
        guards: list[tuple[int, int]],
    ) -> set[int]:
        """
        Args:
            n: Number of nodes in the graph (1-indexed).
            m: Number of edges in the graph.
            k: Number of guards.
            edges: List of edges as (u, v) pairs where u and v are 1-indexed node IDs.
            guards: List of guards as (p, h) pairs where p is the 1-indexed node ID
                   and h is the protection range.

        Returns:
            set[int]: A set of 1-indexed node IDs that are protected.
        """
        e = [[] for _ in range(n)]
        for u, v in edges:
            e[u - 1].append(v - 1)
            e[v - 1].append(u - 1)
        guarded_nodes = [GuardedNode(p - 1, h) for p, h in guards]
        heap = Heap(guarded_nodes)
        result = set()
        while heap:
            current = heap.pop()
            if current.node in result:
                continue
            result.add(current.node)
            if current.h == 0:
                continue
            for neighbor in e[current.node]:
                if neighbor not in result:
                    heap.push(GuardedNode(neighbor, current.h - 1))
        return {node + 1 for node in result}


if __name__ == "__main__":
    """
    Main entry point for the program.

    Reads input in the following format:
    - First line: three integers n, m, k (number of nodes, edges, and guards)
    - Next m lines: two integers u, v representing an edge
    - Next k lines: two integers p, h representing a guard at node p with strength h

    Outputs:
    - First line: the number of protected nodes
    - Second line: the sorted list of protected node IDs
    """
    n, m, k = map(int, input().split())
    edges = []
    for _ in range(m):
        u, v = map(int, input().split())
        edges.append((u, v))
    guarded_nodes = []
    for _ in range(k):
        p, h = map(int, input().split())
        guarded_nodes.append((p, h))
    result = Solution.solve(n, m, k, edges, guarded_nodes)
    print(len(result))
    print(*sorted(result))
```
