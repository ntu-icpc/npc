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

With $$1\leq N,M \leq 500$$, $$O(N \times M)$$ solution are able to pass. Therefore, it is just a classic DP solution.

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


