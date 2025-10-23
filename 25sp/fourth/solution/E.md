---
title: E - Minecraft
layout: page
parent: NPC25 The 4th Contest Solution
---

# E - Minecraft

## Subtask 1
```cpp
#include <algorithm>
#include <iostream>
#include <vector>

class Solution {
 public:
  /**
   * @param n: number of columns
   * @param heights: initial height of each column
   * @return: return an integer representing the minimum number of block added
   * to form a mountain
   */
  static long long solve(int n, std::vector<int>& heights) {
    // Implement your solution by completing the following function
    long long maxv = *max_element(heights.begin(), heights.end());
    long long ans = 1e18;
    for (int idx = 0; idx < n; idx++) {
      long long temp = maxv - heights[idx];
      long long curmax = heights[0];
      for (int i = 1; i < idx; i++) {
        temp += std::max(0ll, curmax - heights[i]);
        curmax = std::max(curmax, (long long)heights[i]);
      }

      curmax = heights[n - 1];
      for (int i = n - 2; i > idx; i--) {
        temp += std::max(0ll, curmax - heights[i]);
        curmax = std::max(curmax, (long long)heights[i]);
      }
      ans = std::min(ans, temp);
    }
    return ans;
  }
};

int main() {
  int n;
  std::cin >> n;
  std::vector<int> heights(n);
  for (int i = 0; i < n; ++i) std::cin >> heights[i];
  long long ans = Solution::solve(n, heights);
  std::cout << ans << "\n";
  return 0;
}
```
## Solution

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

class Solution {
 public:
  /**
   * @param n: number of columns
   * @param heights: initial height of each column
   * @return: return an integer representing the minimum number of block added
   * to form a mountain
   */
  static long long solve(int n, std::vector<int>& heights) {
    // Implement your solution by completing the following function
    std::vector<std::pair<long long, int> > prefix(n), suffix(n);

    prefix[0] = {0, heights[0]};
    for (int i = 1; i < n; i++) {
      prefix[i].first =
          prefix[i - 1].first + std::max(0, prefix[i - 1].second - heights[i]);
      prefix[i].second = std::max(prefix[i - 1].second, heights[i]);
    }

    suffix[n - 1] = {0, heights[n - 1]};
    for (int i = n - 2; i >= 0; i--) {
      suffix[i].first =
          suffix[i + 1].first + std::max(0, suffix[i + 1].second - heights[i]);
      suffix[i].second = std::max(suffix[i + 1].second, heights[i]);
    }

    long long ans = std::min(prefix[n - 1].first, suffix[0].first);
    for (int i = 1; i < n - 1; i++) {
      if (prefix[i].second >= suffix[i].second) {
        ans = std::min(ans, (long long)prefix[i].first + suffix[i + 1].first);
      } else {
        ans = std::min(ans, (long long)prefix[i - 1].first + suffix[i].first);
      }
    }
    return ans;
  }
};

int main() {
  int n;
  std::cin >> n;
  std::vector<int> heights(n);
  for (int i = 0; i < n; ++i) std::cin >> heights[i];
  long long ans = Solution::solve(n, heights);
  std::cout << ans << "\n";
  return 0;
}

```