---
title: B - Distance
layout: page
parent: Solutions
---

# B - Distance

## Solution

We can use binary search to reduce the question to a decision problem:

{: .note-title }
> Abridged Statement (Optimization Problem)
> 
> Choose $$k$$ positions such that the minimum distance between any of the two $$k$$ positions are maximized.

{: .note-title }
> Decision Version
> 
> Given a distance $$D$$, is it possible to select $$k$$ positions, such that the distance between any two positions is at least $$D$$ (i.e. the distance between neighbouring positions is at least $$D$$).

Then, we can propose a greedy algorithm to solve the decision version problem:

1. Sort the positions, set the current latest position as negative infinity ($$-\infty$$)
2. Enumerate the array, if the current position is at least current latest position $$+ D$$, then select the current position and set it to the current latest position.
3. If there is at least $$k$$ position chosen, then the answer is `YES`.

{: .note-title }
> Proof of Correctness
> 
> The greedy algorithm is essentially implying given $$D$$, if the maximum number of positions you can choose with the aforementioned constraint \textbf{is greater than or equal to} $$k$$, then the answer is `YES`.
>
> Instead of giving a full proof of correctness, we show a reduction that reduces the problem into a classical greedy problem where the correctness of the solution is well-known and proven.
>
> For each of the position $$x_i$$, we create an interval $$[x_i, x_i+ D)$$. Then the problem is reduced to finding the maximum number of non-overlapping intervals (which is known as [Activity Selection Problem](https://www.geeksforgeeks.org/activity-selection-problem-greedy-algo-1/)).
>
> The proof of correctness of the problem can be found at Section 15.1 of the book {% cite cormen2022introduction %}.
>
> The optimal strategy for the Activity Selection problem is to sort the interval by end time and apply the same algorithm as our greedy algorithm.
>
> Since in this specially constructed input, the duration of all intervals are the same, therefore, the order of intervals sort by end time is same as the order sort by start time (which is the position).
>
> Therefore, the reduction is correct and thus, our greedy algorithm is correct.

## Codes

{% tabs Code %}

{% tab Code C %}
```c
#include <stdio.h>
#include <stdlib.h>

/**
 * @brief Comparison function for qsort to sort integers in ascending order
 *
 * @param a Pointer to the first element to compare
 * @param b Pointer to the second element to compare
 * @return int Negative if a < b, zero if a == b, positive if a > b
 */
int compare(const void *a, const void *b) { return *(int *)a - *(int *)b; }

/**
 * @brief Checks if it's possible to place k positions with a minimum distance
 * of 'mid'
 *
 * This function uses a greedy approach to check if we can place k positions
 * such that the minimum distance between any two positions is at least 'mid'.
 *
 * @param n The total number of available positions
 * @param k The number of positions we need to select
 * @param x Array of position coordinates
 * @param mid The minimum distance to check
 * @return int 1 if possible, 0 if not possible
 */
int check(int n, int k, int *x, int mid) {
  int count = 1;
  int current_x = x[0];
  for (int i = 1; i < n; i++) {
    if (x[i] - current_x >= mid) {
      count++;
      current_x = x[i];
    }
  }
  return count >= k;
}

/**
 * @brief Solves the maximum minimum distance problem
 *
 * @param n The total number of available positions
 * @param k The number of positions we need to select
 * @param x Array of position coordinates
 * @return int The maximum possible minimum distance
 */
int solve(int n, int k, int *x) {
  qsort(x, n, sizeof(int), compare);
  int l = 0, r = x[n - 1] - x[0];
  while (l < r) {
    int mid = (l + r + 1) / 2;
    if (check(n, k, x, mid)) {
      l = mid;
    } else {
      r = mid - 1;
    }
  }
  return l;
}

int main() {
  int n, k;
  scanf("%d%d", &n, &k);
  int *x = (int *)calloc(n, sizeof(int));
  for (int i = 0; i < n; i++) {
    scanf("%d", &x[i]);
  }
  int result = solve(n, k, x);
  printf("%d", result);
  free(x);
  return 0;
}
```
{% endtab %}

{% tab Code C++ %}
```cpp
#include <algorithm>
#include <iostream>
#include <vector>

class Solution {
 public:
  /**
   * @param n The total number of available positions
   * @param k The number of positions we need to select
   * @param x Vector of position coordinates
   * @return int The maximum possible minimum distance
   */
  static int solve(int n, int k, std::vector<int>& x) {
    std::sort(x.begin(), x.end());
    int l = 1, r = 1000000000, ans = -1;

    while (l < r) {
      int mid = (l + r + 1) / 2;
      int cnt = 1, last = x[0];
      for (int i = 1; i < n; ++i) {
        if (x[i] - last >= mid) {
          cnt++;
          last = x[i];
        }
      }
      if (cnt >= k) {
        l = mid, ans = mid;
      } else {
        r = mid - 1;
      }
    }

    return l;
  }
};

int main() {
  int n, k;
  std::cin >> n >> k;

  std::vector<int> x(n);
  for (int i = 0; i < n; ++i) {
    std::cin >> x[i];
  }

  std::cout << Solution::solve(n, k, x) << std::endl;

  return 0;
}
```
{% endtab %}

{% tab Code Java %}
```java
import java.io.*;
import java.lang.*;
import java.util.*;

class Solution {
  /**
   * @param n: the number of positions
   * @param k: the number of chosen position
   * @param x: array of positions
   * @return: return the maximum minimum distance between any two chosen position
   */
  public static int solve(int n, int k, int[] x) {
    // Implement your solution here
    Arrays.sort(x);
    int l = 1, r = 1000000000;
    while (l < r) {
      int mid = (l + r + 1) / 2;
      int cnt = 1;
      int last = x[0];
      for (int i = 1; i < n; i++) {
        if (x[i] - last >= mid) {
          cnt++;
          last = x[i];
        }
      }
      if (cnt >= k) {
        l = mid;
      } else {
        r = mid - 1;
      }
    }
    return l;
  }

  public static void main(String[] args) {
    Scanner input = new Scanner(System.in);

    int n = input.nextInt();
    int k = input.nextInt();
    int[] x = new int[n];
    for (int i = 0; i < n; i++) {
      x[i] = input.nextInt();
    }
    System.out.println(solve(n, k, x));
  }
}
```
{% endtab %}

{% tab Code Python %}
```python
from typing import List


class Solution:
    @staticmethod
    def check(x: List[int], distance: int, k: int) -> bool:
        """
        Checks if it's possible to place k positions with a minimum distance of 'distance'.

        @param x: List of position coordinates
        @param distance: The minimum distance to check
        @param k: The number of positions we need to select
        @return: True if it's possible, False otherwise
        """
        count = 1
        current_x = x[0]
        for i in range(1, len(x)):
            if x[i] - current_x >= distance:
                count += 1
                current_x = x[i]
        return count >= k

    @staticmethod
    def solve(n: int, k: int, x: List[int]) -> int:
        """
        Determines the maximum possible minimum distance between any two chosen positions.

        @param n: The total number of available positions
        @param k: The number of positions we need to select
        @param x: List of position coordinates
        @return: The maximum possible minimum distance
        """
        x.sort()
        l, r = 0, x[-1] - x[0]
        while l < r:
            mid = (l + r + 1) // 2
            if Solution.check(x, mid, k):
                l = mid
            else:
                r = mid - 1
        return l


if __name__ == "__main__":
    n, k = map(int, input().split())
    x = list(map(int, input().split()))
    print(Solution.solve(n, k, x))
```
{% endtab %}

## References

{% bibliography --cited %}
