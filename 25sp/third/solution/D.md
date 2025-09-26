---
title: D - Knapsack
layout: page
parent: NPC25 Welcome AY25/26 Contest Solution
---

# D - Knapsack

{% tabs ThirdD %}
{% tab ThirdD Python %}
```python
from typing import List

# If you want to use a deep recursion, you need to increase the recursion limit
# Python's default recursion limit is 1000
# Uncomment the following two lines if you need to use deep recursion
# import sys
# sys.setrecursionlimit(10**8)


class Solution:
    @staticmethod
    def solve(n: int, weights: List[int], capacity: int) -> int:
        """
        Args:
            n (int): number of items
            weights (list[int]): array of integers where the i-th index represents the weight of i-th item
            capacity (int): the maximum capacity of knapsack

        Returns:
            int: an integer representing the minimum number of items to choose to fulfiled the constraint
        """
        # Implement your solution here
        weights.sort(reverse=True)
        ans = 0
        sum = 0
        for w in weights:
            sum += w
            ans += 1
            if sum >= (capacity + 1) // 2:
                break

        if sum < (capacity + 1) // 2:
            return -1
        return ans


if __name__ == "__main__":
    n, capacity = tuple(map(int, input().split()))
    weights = list(map(int, input().split()))

    solution = Solution.solve(n, weights, capacity)
    print(solution)
```
{% endtab %}
{% tab ThirdD C %}
```c
#include <stdio.h>
#include <stdlib.h>

/**
 * @param n: number of items
 * @param weights: an array of long integers where the i-th index represents the
 * weight of i-th item
 * @param capacity: a long integer representing the maximum capacity of knapsack
 * @return: return an integer representing the minimum number of items to choose
 * to fulfiled the constraint
 */
int solve(int n, long long *weights, long long capacity) {
  sort(weights, weights + n);
  reverse(weights, weights + n);
  int ans = 0;
  long long sum = 0;
  for (int i = 0; i < n; i++) {
    sum += weights[i];
    ans++;
    if (sum >= (capacity + 1) / 2) break;
  }
  if (sum < (capacity + 1) / 2) ans = -1;
  return ans;
}

int main() {
  int n;
  long long capacity;
  scanf("%d %lld", &n, &capacity);
  long long *weights = (long long *)calloc(n, sizeof(long long));
  for (int i = 0; i < n; ++i) {
    scanf("%lld", &weights[i]);
  }
  int ans = solve(n, weights, capacity);
  printf("%d\n", ans);
  free(weights);
  return 0;
}
```
{% endtab %}
{% tab ThirdD C++ %}
```cpp
#include <iostream>
#include <vector>
using namespace std;
class Solution {
 public:
  /**
   * @param n: number of items
   * @param weights: an array of long integers where the i-th index represents
   * the weight of i-th item
   * @param capacity: a long integer representing the maximum capacity of
   * knapsack
   * @return: return an integer representing the minimum number of items to
   * choose to fulfiled the constraint
   */
  static int solve(int n, std::vector<long long>& weights, long long capacity) {
    // Implement your solution by completing the following function
    sort(weights.rbegin(), weights.rend());
    int ans = 0;
    long long sum = 0;
    for (int i = 0; i < n; i++) {
      sum += weights[i];
      ans++;
      if (sum >= (capacity + 1) / 2) break;
    }
    if (sum < (capacity + 1) / 2) ans = -1;
    return ans;
  }
};

int main() {
  int n;
  long long capacity;
  std::cin >> n >> capacity;
  std::vector<long long> weights(n);
  for (int i = 0; i < n; ++i) std::cin >> weights[i];
  int ans = Solution::solve(n, weights, capacity);
  std::cout << ans << "\n";
  return 0;
}
```
{% endtab %}
{% tab ThirdD Java %}
```java
import java.util.Arrays;
import java.util.Collections;
import java.util.Scanner;

class Solution {
  /**
   * @param n: number of items
   * @param weights: an array of long integers where the i-th index represents the weight of i-th
   *     item
   * @param capacity: a long integer representing the maximum capacity of knapsack
   * @return: return an integer representing the minimum number of items to choose to fulfiled the
   * constraint
   */
  static int solve(int n, long[] weights, long capacity) {
    // Implement your solution here

    Arrays.sort(weights);
    long sum = 0;
    int ans = 0;
    for (int i = n - 1; i >= 0; i--) {
      long w = weights[i];
      sum += w;
      ans += 1;
      if (sum >= (capacity + 1) / 2)
        break;
    }
    if (sum < (capacity + 1) / 2)
      return -1;
    return ans;
  }

  public static void main(String[] args) throws java.lang.Exception {
    Scanner input = new Scanner(System.in);

    int n = input.nextInt();
    long capacity = input.nextLong();

    long[] weights = new long[n];
    for (int i = 0; i < n; i++) {
      weights[i] = input.nextLong();
    }

    int ans = solve(n, weights, capacity);
    System.out.println(ans);

    input.close();
  }
}
```
{% endtab %}
{% endtabs %}