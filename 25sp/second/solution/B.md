---
title: B - Tree 2
layout: page
parent: Solutions
---

# B - Tree 2

## 30 pts? 60 pts!

Idea: for all nodes, "climbing" to the root

![](imgs/B/climb.png)

This algo actually earns 60 pts!

Because the tree generated randomly using $$ p_i = \text{rand}(1, i-1) $$ has an expected depth of $$ \mathcal{O}(\log n) $$.

The proof will be at the end of the solution.

{% tabs Code30 %}
{% tab Code30 C++ %}
```cpp
#include <functional>
#include <iostream>
#include <vector>

class Solution {
 public:
  static std::vector<int> solve(int n, const std::vector<int> &parents) {
    std::vector<int> depth;
    for (int i = 0; i < n; ++i) {
      int current = i;
      int d = 0;
      while (current != 0) {
        current = parents[current - 1] - 1;
        d++;
      }
      depth.push_back(d);
    }
    return depth;
  }
};

int main() {
  std::ios::sync_with_stdio(false);
  std::cin.tie(nullptr);
  std::cout.tie(nullptr);
  int n;
  std::cin >> n;
  std::vector<int> parents;
  parents.reserve(n - 1);
  for (int i = 1; i < n; ++i) {
    int parent;
    std::cin >> parent;
    parents.push_back(parent);
  }
  auto result = Solution::solve(n, parents);
  for (int i = 0; i < n; ++i) {
    std::cout << result[i] << " ";
  }
  return 0;
}
```
{% endtab %}
{% endtabs %}

## 100 pts

DFS and BFS is a good idea for most of the tree problem, because this method allows the entire tree to be traversed in a specific order.

In both types of search, the parent node always arrives before the child nodes.

So, if we let $$ d_i $$ be the distance of node $$ i $$, during our DFS/BFS, $$ d_{p_i} $$ is always computed before $$ d_i $$.

Here is the pseudocode for DFS:

![](imgs/B/dfs.png)

Here is the pseudocode for BFS:

![](imgs/B/bfs.png)

{% tabs Code100 %}
{% tab Code100 Python %}
```python
import sys

sys.setrecursionlimit(10**8)


class Solution:
    @staticmethod
    def solve(n: int, parents: list[int]) -> list[int]:
        children = [[] for _ in range(n)]
        for i in range(1, n):
            children[parents[i - 1] - 1].append(i)
        depth = [0] * n

        def dfs(u: int):
            for v in children[u]:
                depth[v] = depth[u] + 1
                dfs(v)

        dfs(0)
        return depth


if __name__ == "__main__":
    n = int(input())
    parents = list(map(int, input().split()))
    depth = Solution.solve(n, parents)
    print(*depth)
```
{% endtab %}
{% tab Code100 C %}
```c
#include <stdio.h>
#include <stdlib.h>

void solve(int n, int *parents, int *depth);

int main() {
  int n;
  scanf("%d", &n);
  int *parents = (int *)calloc(n - 1, sizeof(int));
  for (int i = 1; i < n; ++i) {
    scanf("%d", &parents[i - 1]);
  }
  int *depth = (int *)calloc(n, sizeof(int));
  solve(n, parents, depth);
  for (int i = 0; i < n; ++i) {
    printf("%d ", depth[i]);
  }
  printf("\n");
  free(depth);
  free(parents);
  return 0;
}

#define INIT_CAPACITY 5

struct ArrayList {
  int num;
  int capacity;
  int *data;
};

void array_list_init(struct ArrayList *array_list) {
  array_list->num = 0;
  array_list->capacity = INIT_CAPACITY;
  array_list->data = (int *)calloc(array_list->capacity, sizeof(int));
}

void array_list_add(struct ArrayList *array_list, int value) {
  if (array_list->num == array_list->capacity) {
    array_list->capacity *= 2;
    array_list->data =
        (int *)realloc(array_list->data, array_list->capacity * sizeof(int));
  }
  array_list->data[array_list->num++] = value;
}

void array_list_free(struct ArrayList *array_list) { free(array_list->data); }

#undef INIT_CAPACITY

void dfs(int u, struct ArrayList *children, int *depth) {
  for (int i = 0; i < children[u].num; ++i) {
    depth[children[u].data[i]] = depth[u] + 1;
    dfs(children[u].data[i], children, depth);
  }
}

void solve(int n, int *parents, int *depth) {
  struct ArrayList *children =
      (struct ArrayList *)calloc(n, sizeof(struct ArrayList));
  for (int i = 0; i < n; ++i) {
    array_list_init(&children[i]);
  }
  for (int i = 1; i < n; ++i) {
    array_list_add(&children[parents[i - 1] - 1], i);
  }
  dfs(0, children, depth);
  for (int i = 0; i < n; ++i) {
    array_list_free(&children[i]);
  }
  free(children);
}
```
{% endtab %}
{% tab Code100 C++ %}
```cpp
#include <functional>
#include <iostream>
#include <vector>

class Solution {
 public:
  static std::vector<int> solve(int n, const std::vector<int> &parents) {
    std::vector<std::vector<int>> children(n);
    for (int i = 1; i < n; ++i) {
      children[parents[i - 1] - 1].push_back(i);
    }
    std::vector<int> depth(n, 0);
    std::function<void(int)> dfs = [&](int u) {
      for (int v : children[u]) {
        depth[v] = depth[u] + 1;
        dfs(v);
      }
    };
    dfs(0);
    return depth;
  }
};

int main() {
  std::ios::sync_with_stdio(false);
  std::cin.tie(nullptr);
  std::cout.tie(nullptr);
  int n;
  std::cin >> n;
  std::vector<int> parents;
  parents.reserve(n - 1);
  for (int i = 1; i < n; ++i) {
    int parent;
    std::cin >> parent;
    parents.push_back(parent);
  }
  auto result = Solution::solve(n, parents);
  for (int i = 0; i < n; ++i) {
    std::cout << result[i] << " ";
  }
  std::cout << std::endl;
  return 0;
}
```
{% endtab %}
{% tab Code100 Java %}
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class Solution {
  static void dfs(int u, List<Integer>[] children, int[] depth) {
    for (int v : children[u]) {
      depth[v] = depth[u] + 1;
      dfs(v, children, depth);
    }
  }

  static int[] solve(int n, int[] parents) {
    int[] depth = new int[n];
    @SuppressWarnings("unchecked") List<Integer>[] children = new ArrayList[n];
    for (int i = 0; i < n; i++) {
      children[i] = new ArrayList<>();
    }
    for (int i = 1; i < n; i++) {
      children[parents[i - 1] - 1].add(i);
    }
    dfs(0, children, depth);
    return depth;
  }

  public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    int n = scanner.nextInt();
    int[] parents = new int[n - 1];
    for (int i = 1; i < n; i++) {
      parents[i - 1] = scanner.nextInt();
    }
    int[] depth = solve(n, parents);
    for (int i = 0; i < n; i++) {
      System.out.print(depth[i] + " ");
    }
    scanner.close();
  }
}
```
{% endtab %}
{% endtabs %}

## Proof of expected depth of $$ \mathcal{O}(\log n) $$

Let

$$
d_i = \mathbb{E}[\text{depth of node } i]
$$

As $$ p_i \sim \text{uniform}(1, i-1) $$, we can have

$$
\begin{aligned}
d_i &= \mathbb{E}_{p\sim \text{uniform}(1, i-1)}\left[d_{p} + 1\right] \\
        &= \mathbb{E}_{p\sim \text{uniform}(1, i-1)}\left[d_{p}\right] + 1 \\
        &= 1 + \frac{1}{i-1}\sum_{p=1}^{i-1}d_p\\
        &=1 + \frac{1}{i-1} d_{i-1} + \frac{1}{i-1} \sum_{p=1}^{i-2}d_p
\end{aligned}
$$

As

$$
d_i = 1 + \frac{1}{i-1}\sum_{p=1}^{i-1}d_p
$$

so

$$
\sum_{p=1}^{i-1}d_p = (i-1)(d_p-1)
$$

Thus

$$
\begin{aligned}
d_i&=1 + \frac{1}{i-1} d_{i-1} + \frac{1}{i-1} \sum_{p=1}^{i-2}d_p\\
        &=1 + \frac{1}{i-1} d_{i-1} + \frac{1}{i-1}\cdot (i-2) (d_{i-1} - 1)\\
        &=\frac{1}{i-1} + d_{i-1} = \sum_{j=1}^{i-1}\frac{1}{i}
    \end{aligned}
$$

So

$$
\mathbb{E}[\text{depth of node } i]=\sum_{j=1}^{i-1}\frac{1}{i}
$$

And this is a very famous formula called [Harmonic series](https://en.wikipedia.org/wiki/Harmonic_series_(mathematics)).

It can be easily proved that

$$
\mathbb{E}[\text{depth of node } i]=\sum_{j=1}^{i-1}\frac{1}{i}\le 1 + \int_1^{i-1}\frac{1}{x}\mathrm{d}x=1+\log (i-1)
$$
