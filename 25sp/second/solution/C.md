---
title: C - Subsequence 2
layout: page
parent: Solutions
---

# C - Subsequence 2

## Abridged Problem Statement

Given two non-descending arrays, find the Longest Common Subsequence (LCS) between the two arrays

Subtasks:

- Subtask 1: $$1 \leq N,M \leq 20$$
- Subtask 2: $$1 \leq N,M \leq 500$$
- Subtask 3: $$1 \leq N,M \leq 10000$$
- Subtask 4: $$1 \leq N,M \leq 100000$$

## Solution

### Subtask 1: Complete Search

#### Idea

Do a complete search (brute force) over all the possible subsequences of array $$A$$.

During the search, an array $$C$$ is constructed. To verify that array $$C$$ is a subsequence of array $$B$$, you may enumerate the array $$B$$ and maintain a "pointer" on array $$C$$.

During the enumeration of array $$B$$, if the current element is same as the element the pointer is pointing to at array $$C$$, increment the pointer.

#### Implementation and Analysis

To implement the algorithm, you may write a **recursive function** (backtracking). Alternatively, you may use a bitmask to write an **iterative function**. Please check the code for the implementation.

The time complexity is as such:

- There are $$2^N$$ possible subsequences (Each element has two possibilities - chosen in the subsequence or removed from the array).
- To verify whether each potential subsequence is a subsequence of $$B$$, you have to do an $$\mathcal{O}(N + M)$$ enumeration. That is $$\mathcal{O}(N)$$ for the enumeration on the subsequence $$C$$ and $$\mathcal{O}(M)$$ for the enumeration on the array $$B$$.
- The total time complexity is $$\mathcal{O}(2^{N} \cdot (N + M))$$

{% tabs Subtask1 %}
{% tab Subtask1 C++ %}
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  int n, m;
  cin >> n >> m;

  vector<int> a(n), b(m);
  for (auto& i : a) cin >> i;
  for (auto& i : b) cin >> i;
  int ans = 0;
  for (int msk = 0; msk < (1 << n); msk++) {
    int j = 0;
    bool can = true;
    int len = 0;
    for (int i = 0; i < n; i++) {
      if (msk & (1 << i)) {
        len++;
        while (j < m && a[i] != b[j]) j++;
        if (j == m) {
          can = false;
          break;
        }
      }
    }
    if (can) ans = max(ans, len);
  }
  cout << ans << '\n';
}
```
{% endtab %}
{% endtabs %}

### Subtask 2: Dynamic Programming

#### Idea

With $$1\leq N,M \leq 500$$, $$\mathcal{O}(N \times M)$$ solution are able to pass. Therefore, it is just a classic DP solution.

#### Implementation


$$DP[i][j]$$ stores the length of LCS of the array $$[a_0, a_1, \cdots, a_{i-1}]$$ and the array $$[a_0, a_1, \cdots, a_{j-1}]$$.

The transition between the states is as follows:

- $$DP[i][j] = DP[i - 1][j - 1]$$, if $$a_i == a_j$$ and $$i \geq 1, j\geq 1$$.
- $$DP[i][j] = \max(DP[i - 1][j - 1], DP[i][j - 1])$$, otherwise (if $$i-1$$ or $$j-1$$ would be negative, just ignore it). 

The base state $$DP[0][0] = 0$$.

The answer is $$DP[N][M]$$. You may build a bottom-up DP or top-down DP.

{% tabs Subtask2 %}
{% tab Subtask2 C++ %}
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  int n, m;
  cin >> n >> m;
  vector<int> a(n), b(m);
  for (auto& i : a) cin >> i;
  for (auto& i : b) cin >> i;

  int dp[n + 1][m + 1];
  memset(dp, 0, sizeof(dp));
  dp[0][0] = 0;
  for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
      if (a[i] == b[j])
        dp[i + 1][j + 1] = dp[i][j] + 1;
      else
        dp[i + 1][j + 1] = max(dp[i + 1][j], dp[i][j + 1]);
    }
  }
  cout << dp[n][m] << "\n";
}
```
{% endtab %}
{% endtabs %}

### Subtask 3: Rolling DP

#### Motivation

With $$1\leq N, M\leq 10000$$, $$\mathcal{O}(N \times M)$$ solution are still able to pass. However, if the two-dimensional DP array is constructed with size $$N\times M$$, your solution would get Memory Limit Exceeded (MLE). This is because the space complexity is $$\mathcal{O}(N\times M)$$.

From SC1006, if the DP arrays store 32-bit integers, what is the maximum number of bytes used by the DP array? 

32-bit is equivalent to 4 bytes. If $$N = M = 10000$$, the number of bytes is $$N \times M \times 4 = 4 \times 10^8 B \approx 381 MB$$. 

The memory limit is $$512 MB$$. Therefore, including some required memory storage (TODO how much), the DP array would use too much memory space.

The idea is to use "Rolling DP" to compress the space to $$\mathcal{O}(N + M)$$.

#### Implementation

Notice that, for any $$i$$, all the DP with first dimension is $$i$$ only depends on the DP with first dimension is $$i - 1$$. That is, given $$i$$, $$\forall j \in [0,M]$$, $$DP[i][j]$$ only depends on $$DP[i - 1][j]$$ or $$DP[i - 1][j - 1]$$.

If bottom-up DP is used (Nested Loop of $$i$$ followed by $$j$$), it is not required to store any states with first dimension $$\leq i-2$$.

Therefore, the transition is as follows:

- $$DP[i \% 2][j] = DP[(i - 1) \% 2][j - 1]$$, if $$a_i == a_j$$ and $$i \geq 1, j\geq 1$$.
- $$DP[i \% 2][j] = \max(DP[(i - 1) \% 2][j], DP[i \% 2][j - 1])$$, otherwise (if $$i-1$$ or $$j-1$$ would be negative, just ignore it). 

The answer is $$DP[N\%2][M]$$.

{% tabs Subtask3 %}
{% tab Subtask3 C++ %}
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  int n, m;
  cin >> n >> m;
  vector<int> a(n), b(m);
  for (auto& i : a) cin >> i;
  for (auto& i : b) cin >> i;

  int dp[2][m + 1];
  memset(dp, 0, sizeof(dp));
  dp[0][0] = 0;
  for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
      if (a[i] == b[j])
        dp[(i + 1) % 2][j + 1] = dp[i % 2][j] + 1;
      else
        dp[(i + 1) % 2][j + 1] = max(dp[(i + 1) % 2][j], dp[i % 2][j + 1]);
    }
  }
  cout << dp[n % 2][m] << "\n";
}
```
{% endtab %}
{% endtabs %}

### Subtask 4: Two Pointers

$$1\leq N,M \leq 100000$$. $$\mathcal{O}(NM)$$ solution would TLE.

The idea is to take advantage that arrays $$A$$ and $$B$$ are in ascending order.

For a given $$i$$ and $$j$$, if $$a_i < b_j$$ and denote $$i' > i$$ as the first index such that $$a_{i'} \geq b_j$$, we know that for all $$j'' \geq j$$ and $$i \leq i'' \leq i'$$, $$DP[i''][j''] = DP[i][j]$$.

With this, we do not need to compute the answer to many DP states. We just need to increment $$i$$ to $$i'$$ and compute the solution. Likewise, if $$a_i > b_j$$, we increment $$j$$ until $$a_i \leq b_j$$.

If $$a_i = b_j$$, then we increment both $$i$$ and $$j$$ by one and add 1 to the answer.

This leads to a two pointer algorithm.

{% tabs Subtask4 %}
{% tab Subtask4 Python %}
```python
class Solution:
    @staticmethod
    def solve(n: int, m: int, a: list[int], b: list[int]) -> int:
        ans = 0
        x = y = 0
        while x < n and y < m:
            if a[x] == b[y]:
                ans += 1
                x += 1
                y += 1
            elif a[x] < b[y]:
                x += 1
            else:
                y += 1
        return ans


if __name__ == "__main__":
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    print(Solution.solve(n, m, a, b))
```
{% endtab %}
{% tab Subtask4 C %}
```c
#include <stdio.h>
#include <stdlib.h>

int solve(int n, int m, int *a, int *b) {
  int p1 = 0, p2 = 0, ans = 0;
  while (p1 != n && p2 != m) {
    if (a[p1] == b[p2]) {
      ans++;
      p1++;
      p2++;
    } else if (a[p1] < b[p2]) {
      p1++;
    } else {
      p2++;
    }
  }
  return ans;
}

int main() {
  int n, m;
  scanf("%d%d", &n, &m);
  int *a = (int *)calloc(n, sizeof(int));
  int *b = (int *)calloc(m, sizeof(int));
  for (int i = 0; i < n; i++) {
    scanf("%d", &a[i]);
  }
  for (int i = 0; i < m; i++) {
    scanf("%d", &b[i]);
  }
  int result = solve(n, m, a, b);
  printf("%d\n", result);
  free(a);
  free(b);
  return 0;
}
```
{% endtab %}
{% tab Subtask4 C++ %}
```cpp
#include <iostream>
#include <vector>

class Solution {
 public:
  static int solve(int n, int m, std::vector<int> a, std::vector<int> b) {
    int p1 = 0, p2 = 0, ans = 0;
    while (p1 != n && p2 != m) {
      if (a[p1] == b[p2]) {
        ans++;
        p1++;
        p2++;
      } else if (a[p1] < b[p2]) {
        p1++;
      } else {
        p2++;
      }
    }
    return ans;
  }
};

int main() {
  int n, m;
  std::cin >> n >> m;
  std::vector<int> a(n), b(m);
  for (int i = 0; i < n; i++) {
    std::cin >> a[i];
  }
  for (int i = 0; i < m; i++) {
    std::cin >> b[i];
  }
  std::cout << Solution::solve(n, m, a, b) << std::endl;
  return 0;
}
```
{% endtab %}
{% tab Subtask4 Java %}
```java
import java.util.Scanner;
class Solution {
  public static int solve(int n, int m, int[] a, int[] b) {
    int p1 = 0, p2 = 0, ans = 0;
    while (p1 != n && p2 != m) {
      if (a[p1] == b[p2]) {
        ans++;
        p1++;
        p2++;
      } else if (a[p1] < b[p2]) {
        p1++;
      } else {
        p2++;
      }
    }
    return ans;
  }

  public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    int n = scanner.nextInt();
    int m = scanner.nextInt();
    int[] a = new int[n];
    int[] b = new int[m];
    for (int i = 0; i < n; i++) {
      a[i] = scanner.nextInt();
    }
    for (int i = 0; i < m; i++) {
      b[i] = scanner.nextInt();
    }
    System.out.println(solve(n, m, a, b));
    scanner.close();
  }
}
```
{% endtab %}
{% tab Subtask4 Go %}
```go
package main

import "fmt"

func solve(n int, m int, a []int, b []int) int {
	p1 := 0
	p2 := 0
	ans := 0
	for p1 < n && p2 < m {
		if a[p1] == b[p2] {
			ans++
			p1++
			p2++
		} else if a[p1] < b[p2] {
			p1++
		} else {
			p2++
		}
	}
	return ans
}

func main() {
	var n, m int
	fmt.Scan(&n, &m)
	a := make([]int, n)
	b := make([]int, m)
	for i := 0; i < n; i++ {
		fmt.Scan(&a[i])
	}
	for i := 0; i < m; i++ {
		fmt.Scan(&b[i])
	}
	fmt.Println(solve(n, m, a, b))
}
```
{% endtab %}
{% endtabs %}
