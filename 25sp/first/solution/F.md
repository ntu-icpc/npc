---
title: F - Subsequence
layout: page
parent: Solutions
---

# F - Subsequence

## Codes

{% tabs Code %}
{% tab Code C %}
```c
#include <stdio.h>
#include <stdlib.h>

int compare(const void *a, const void *b) { return *(int *)a - *(int *)b; }

int min(int a, int b) { return a < b ? a : b; }

/**
 * @param n: number of elements
 * @param k: size of the subsequence
 * @param a: array of integers
 * @return: return the minimum difference between max and min elements in any
 * subsequence of size k
 */
int solve(int n, int k, int *a) {
  qsort(a, n, sizeof(int), compare);
  int min_diff = a[k - 1] - a[0];
  for (int i = 0; i <= n - k; i++) {
    min_diff = min(min_diff, a[i + k - 1] - a[i]);
  }
  return min_diff;
}

int main() {
  int n, k;
  scanf("%d %d", &n, &k);
  int *a = (int *)calloc(n, sizeof(int));
  for (int i = 0; i < n; i++) {
    scanf("%d", &a[i]);
  }
  printf("%d\n", solve(n, k, a));
  free(a);
  return 0;
}
```
{% endtab %}

{% tab Code C++ %}
```cpp
#include <algorithm>
#include <climits>
#include <iostream>
#include <vector>

class Solution {
 public:
  /**
   * @param n: number of elements
   * @param k: size of the subsequence
   * @param a: array of integers
   * @return: return the minimum difference between max and min elements in any
   * subsequence of size k
   */
  static int solve(int n, int k, std::vector<int>& a) {
    std::sort(a.begin(), a.end());
    int ans = INT_MAX;
    for (int i = 0; i <= n - k; ++i) {
      ans = std::min(ans, a[i + k - 1] - a[i]);
    }
    return ans;
  }
};

int main() {
  int n, k;
  std::cin >> n >> k;
  std::vector<int> a(n);
  for (int i = 0; i < n; ++i) {
    std::cin >> a[i];
  }

  std::cout << Solution::solve(n, k, a) << std::endl;

  return 0;
}
```
{% endtab %}

{% tab Code Java %}
```java
import java.io.*;
import java.lang.*;
import java.util.*;

class Main {
  /**
   * @param n: number of elements
   * @param k: size of the subsequence
   * @param a: array of integers
   * @return: return the minimum difference between max and min elements in any
   * subsequence of size k
   */
  public static int solve(int n, int k, int[] a) {
    // Implement your solution here
    Arrays.sort(a);
    int ans = Integer.MAX_VALUE;
    for (int i = 0; i <= n - k; i++) {
      ans = Math.min(ans, a[i + k - 1] - a[i]);
    }
    return ans;
  }

  public static void main(String[] args) throws java.lang.Exception {
    Scanner input = new Scanner(System.in);

    int n = input.nextInt();
    int k = input.nextInt();
    int[] a = new int[n];
    for (int i = 0; i < n; i++) {
      a[i] = input.nextInt();
    }

    System.out.println(solve(n, k, a));
    input.close();
  }
}
```
{% endtab %}

{% tab Code Python %}
```python
class Solution:
    @staticmethod
    def solve(n: int, k: int, a: list[int]) -> int:
        """
        Finds the minimum difference between max and min elements in any subsequence of size k.

        Args:
            n (int): Number of elements in the array.
            k (int): Size of the subsequence.
            a (list[int]): Array of integers.

        Returns:
            int: Minimum difference between max and min elements in any subsequence of size k.
        """
        a.sort()
        return min(a[i + k - 1] - a[i] for i in range(n - k + 1))


if __name__ == "__main__":
    n, k = map(int, input().split())
    a = [int(input()) for _ in range(n)]
    print(Solution.solve(n, k, a))
```
{% endtab %}
{% endtabs %}
