---
title: E - Stones
layout: page
parent: NPC25 Welcome AY25/26 Contest Solution
---

# E - Stones

{% tabs ThirdE %}
{% tab ThirdE Python %}
```python
from collections import deque
from typing import List

# If you want to use a deep recursion, you need to increase the recursion limit
# Python's default recursion limit is 1000
# Uncomment the following two lines if you need to use deep recursion
# import sys
# sys.setrecursionlimit(10**8)


class Solution:
    @staticmethod
    def solve(n: int, positions: List[tuple[int, int]]) -> int:
        """
        Args:
            n (int): number of stones
            positions (list[tuple[int,int]]): an array consisting n tuple of integers where the tuple at i-th index represents the xy-coordinate of the i-th stone (The first (resp. second) integer in the pair is the x-coordinate (resp. y-coordinate))

        Returns:
            int: an integer represent the maximum number of stones that can be removed
        """
        xmap = {}
        ymap = {}
        id = 0
        for x, y in positions:
            if xmap.get(x) is None:
                xmap[x] = []
            if ymap.get(y) is None:
                ymap[y] = []

            xmap[x].append(id)
            ymap[y].append(id)
            id += 1

        graph = [[] for _ in range(n)]
        for k in xmap.keys():
            for i in range(1, len(xmap[k])):
                u = xmap[k][i - 1]
                v = xmap[k][i]
                graph[u].append(v)
                graph[v].append(u)
        for k in ymap.keys():
            for i in range(1, len(ymap[k])):
                u = ymap[k][i - 1]
                v = ymap[k][i]
                graph[u].append(v)
                graph[v].append(u)

        ans = 0
        visited = [0 for i in range(n)]
        for i in range(n):
            if visited[i] == 1:
                continue
            ans += 1
            q = deque()
            q.append(i)
            while len(q) != 0:
                u = q.popleft()
                if visited[u] == 1:
                    continue
                visited[u] = 1

                for v in graph[u]:
                    if visited[v] == 1:
                        continue
                    q.append(v)
        return n - ans


if __name__ == "__main__":
    n = int(input())
    positions = [tuple(map(int, input().split())) for i in range(n)]

    solution = Solution.solve(n, positions)
    print(solution)
```
{% endtab %}
<!-- {% tab ThirdE C %}
```c
#include <stdio.h>
#include <stdlib.h>

/**
 * @param n: number of stones
 * @param x: an array consisting n integers where the i-th integer represents
 * the x-coordinate of the i-th stone
 * @param y: an array consisting n integers where the i-th integer represents
 * the y-coordinate of the i-th stone
 * @return: return an integer represent the maximum number of stones that can be
 * removed
 */
int solve(int n, int *x, int *y) {
  // TODO: Implement the solution
  int ans = 0;
  return ans;
}

int main() {
  int n;
  scanf("%d", &n);
  int *x = (int *)calloc(n, sizeof(int));
  int *y = (int *)calloc(n, sizeof(int));
  for (int i = 0; i < n; ++i) {
    scanf("%d %d", &x[i], &y[i]);
  }
  int ans = solve(n, x, y);
  printf("%d\n", ans);
  free(x);
  free(y);
  return 0;
}
```
{% endtab %} -->
{% tab ThirdE C++ %}
```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
using namespace std;

class Solution {
 public:
  /**
   * @param n: number of stones
   * @param positions: an array consisting n pair of integers where the pair at
   * i-th index represents the xy-coordinate of the i-th stone (The first (resp.
   * second) integer in the pair is the x-coordinate (resp. y-coordinate))
   * @return: return an integer represent the maximum number of stones that can
   * be removed
   */
  static int solve(int n, std::vector<std::pair<int, int> >& positions) {
    // Implement your solution by completing the following function
    unordered_map<int, vector<int> > row2stones;
    unordered_map<int, vector<int> > col2stones;
    vector<pair<int, int> > stones;
    cin >> n;
    for (int i = 0; i < n; i++) {
      int x, y;
      cin >> x >> y;
      stones.emplace_back(x, y);

      if (row2stones.find(x) == row2stones.end()) row2stones[x] = vector<int>();
      if (col2stones.find(y) == col2stones.end()) col2stones[y] = vector<int>();
      row2stones[x].emplace_back(i);
      col2stones[y].emplace_back(i);
    }

    vector<vector<int> > adjlist(n);
    for (auto& [row, vec] : row2stones) {
      for (int i = 1; i < vec.size(); i++) {
        adjlist[vec[i]].emplace_back(vec[i - 1]);
        adjlist[vec[i - 1]].emplace_back(vec[i]);
      }
    }
    for (auto& [col, vec] : col2stones) {
      for (int i = 1; i < vec.size(); i++) {
        adjlist[vec[i]].emplace_back(vec[i - 1]);
        adjlist[vec[i - 1]].emplace_back(vec[i]);
      }
    }

    vector<int> visited(n, 0);

    int ncc = 0;
    for (int i = 0; i < n; i++) {
      if (visited[i]) continue;
      ncc++;

      queue<int> q;
      q.emplace(i);
      while (!q.empty()) {
        int u = q.front();
        q.pop();
        if (visited[u]) continue;
        visited[u] = 1;

        for (int v : adjlist[u]) {
          if (visited[v]) continue;
          q.emplace(v);
        }
      }
    }

    return n - ncc;
  }
};

int main() {
  int n;
  std::cin >> n;
  std::vector<std::pair<int, int> > positions(n);
  for (int i = 0; i < n; ++i) {
    int x, y;
    std::cin >> x >> y;
    positions[i] = {x, y};
  }
  int ans = Solution::solve(n, positions);
  std::cout << ans << "\n";
  return 0;
}
```
{% endtab %}
{% tab ThirdE Java %}
```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

class Solution {
  /**
   * @param n: number of stones
   * @param x: an array consisting n integers where the i-th integer represents the x-coordinate of
   *     the i-th stone
   * @param y: an array consisting n integers where the i-th integer represents the y-coordinate of
   *     the i-th stone
   * @return: return an integer represent the maximum number of stones that can be removed
   */
  static int solve(int n, int[] X, int[] Y) {
    // Implement your solution here
    HashMap<Integer, ArrayList<Integer> > xmap = new HashMap<>();
    HashMap<Integer, ArrayList<Integer> > ymap = new HashMap<>();

    for (int i = 0; i < n; i++) {
      if (!xmap.containsKey(X[i]))
        xmap.put(X[i], new ArrayList<>());
      if (!ymap.containsKey(Y[i]))
        ymap.put(Y[i], new ArrayList<>());

      xmap.get(X[i]).add(i);
      ymap.get(Y[i]).add(i);
    }

    ArrayList<ArrayList<Integer> > adjList = new ArrayList<>(n);
    for (int i = 0; i < n; i++) adjList.add(new ArrayList<Integer>());
    for (int k : xmap.keySet()) {
      for (int i = 1; i < xmap.get(k).size(); i++) {
        int u = xmap.get(k).get(i - 1);
        int v = xmap.get(k).get(i);

        adjList.get(u).add(v);
        adjList.get(v).add(u);
      }
    }
    for (int k : ymap.keySet()) {
      for (int i = 1; i < ymap.get(k).size(); i++) {
        int u = ymap.get(k).get(i - 1);
        int v = ymap.get(k).get(i);

        adjList.get(u).add(v);
        adjList.get(v).add(u);
      }
    }

    int[] visited = new int[n];
    for (int i = 0; i < n; i++) visited[i] = 0;
    int ans = 0;
    for (int i = 0; i < n; i++) {
      if (visited[i] != 0)
        continue;
      ans += 1;
      Queue<Integer> q = new LinkedList<Integer>();
      q.add(i);
      visited[i] = 1;
      while (q.size() != 0) {
        int u = q.peek();
        q.remove();

        for (int id : adjList.get(u)) {
          if (visited[id] == 1)
            continue;
          visited[id] = 1;
          q.add(id);
        }
      }
    }
    return n - ans;
  }

  public static void main(String[] args) throws java.lang.Exception {
    Scanner input = new Scanner(System.in);

    int n = input.nextInt();

    int[] x = new int[n];
    int[] y = new int[n];
    for (int i = 0; i < n; i++) {
      x[i] = input.nextInt();
      y[i] = input.nextInt();
    }

    int ans = solve(n, x, y);
    System.out.println(ans);

    input.close();
  }
}
```
{% endtab %}
{% endtabs %}