---
title: H - Bye Bye
layout: page
parent: Solutions
---

# H - Bye Bye

## Solution

Let's consider the people living in $$ x_1 $$ and $$ x_n $$, that is, the residents of the leftmost and rightmost apartments.

WLOG, we assume $$ p_1 \ge p_n $$. Let's think about what the person living in $$ x_n $$ is considering.

First, they will realize that they will be the last one to be dropped off no matter what. Even if there are a large number of people at $$ x_{n-1} $$, causing the vehicle to keep moving right, once the vehicle reaches $$ x_{n-1} $$, all the residents from $$ x_1 $$ to $$ x_{n-2} $$ will unanimously vote to go left. This is because by doing so, the bus will continue moving left, and except for the person at $$ x_n $$, it will head straight in the direction that everyone else desires.

At this point, the clever NTU students will realize that the residents of $$ x_n $$ unanimously hope that the residents of $$ x_1 $$ get home as soon as possible. This is because once the residents of $$ x_1 $$ have reached home, the bus will only move to the right -- no one would choose to go left and send the bus into an uninhabited area.

In other words, they have accepted their fate of being the last to arrive. However, once the bus reaches $$ x_1 $$, they only need to wait for a duration of $$ x_n - x_1 $$ to get home. Since their fate is fixed once the bus reaches $$ x_1 $$, all they need to do is strive to reach $$ x_1 $$ as quickly as possible.

So, can we think that the residents of $$ x_n $$ first let themselves "temporarily stay" at $$ x_1 $$, and then we recursively solve the subproblem? Once the subproblem is solved, we can then direct the bus toward $$ x_n $$.

In other words, the original problem is $$ \left<(x_1, \dots, x_n), (p_1, \dots, p_n)\right> $$. Now, it becomes $$ \left<(x_1, \dots, x_{n-1}), (p_1 + p_n, p_2, \dots, p_{n-1})\right> $$. After solving this subproblem, we add the time to reach $$ x_n $$ to the final answer, which will give the solution to the original problem.

Similarly, if $$ p_1 < p_n $$, then the residents of $$ x_1 $$ will do everything they can to make the bus reach $$ x_n $$ quickly.

## Codes

{% tabs Code %}
{% tab Code C %}
```c
#include <inttypes.h>
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>

typedef struct {
  int position;
  int64_t time;
} Result;

Result solve_range(int s, int *x, int64_t *p, int l, int r) {
  if (s < x[l]) {
    return (Result){x[r], x[r] - s};
  } else if (s > x[r]) {
    return (Result){x[l], s - x[l]};
  } else if (p[l] >= p[r]) {
    p[l] += p[r];
    Result result = solve_range(s, x, p, l, r - 1);
    return (Result){x[r], result.time + x[r] - result.position};
  } else {
    p[r] += p[l];
    Result result = solve_range(s, x, p, l + 1, r);
    return (Result){x[l], result.time + result.position - x[l]};
  }
}

/**
 * Calculate the total time required for all employees to reach their
 * apartments.
 *
 * @param n Number of apartments
 * @param s Initial position of the bus (NTU office location)
 * @param x List of apartment positions on the number line
 * @param p List of number of employees in each apartment
 * @return The total time (in seconds) required for all employees to reach their
 * apartments, since the answer can be large, return it as int64_t
 */
int64_t solve(int n, int s, int *x, int *p) {
  int64_t *p_long = (int64_t *)calloc(n, sizeof(int64_t));
  for (int i = 0; i < n; i++) {
    p_long[i] = (int64_t)p[i];
  }
  Result result = solve_range(s, x, p_long, 0, n - 1);
  free(p_long);
  return result.time;
}

int main() {
  int n, s;
  scanf("%d%d", &n, &s);
  int *x = (int *)calloc(n, sizeof(int));
  int *p = (int *)calloc(n, sizeof(int));
  for (int i = 0; i < n; i++) {
    scanf("%d%d", &x[i], &p[i]);
  }
  printf("%" PRId64 "\n", solve(n, s, x, p));
  free(x);
  free(p);
  return 0;
}
```
{% endtab %}

{% tab Code C++ %}
```cpp
#include <cstdint>
#include <iostream>
#include <vector>

std::pair<int, int64_t> solve_range(int s, std::vector<int> &x,
                                    std::vector<int64_t> &p, int l, int r) {
  if (s < x[l]) {
    return {x[r], x[r] - s};
  } else if (s > x[r]) {
    return {x[l], s - x[l]};
  } else if (p[l] >= p[r]) {
    p[l] += p[r];
    auto [pos, time] = solve_range(s, x, p, l, r - 1);
    return {x[r], time + x[r] - pos};
  } else {
    p[r] += p[l];
    auto [pos, time] = solve_range(s, x, p, l + 1, r);
    return {x[l], time + pos - x[l]};
  }
}

/**
 * Calculate the total time required for all employees to reach their
 * apartments.
 *
 * @param n Number of apartments
 * @param s Initial position of the bus (NTU office location)
 * @param x List of apartment positions on the number line
 * @param p List of number of employees in each apartment
 * @return The total time (in seconds) required for all employees to reach their
 * apartments, since the answer can be large, return it as int64_t
 */
int64_t solve(int n, int s, std::vector<int> &x, std::vector<int> &p) {
  std::vector<int64_t> p_long(n);
  for (int i = 0; i < n; i++) {
    p_long[i] = static_cast<int64_t>(p[i]);
  }
  return solve_range(s, x, p_long, 0, n - 1).second;
}

int main() {
  int n, s;
  std::cin >> n >> s;
  std::vector<int> x;
  std::vector<int> p;
  for (int i = 0; i < n; i++) {
    int x_i, p_i;
    std::cin >> x_i >> p_i;
    x.push_back(x_i);
    p.push_back(p_i);
  }
  std::cout << solve(n, s, x, p) << std::endl;
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
  public Solution() {}
  long[] x;
  long[] p;

  long ans = 0;
  long cur = 0;

  public void recur(int l, int r) {
    if (x[l] > cur) {
      ans += x[r] - cur;
      cur = x[r];
      return;
    }
    if (x[r] < cur) {
      ans += cur - x[l];
      cur = x[l];
      return;
    }

    if (p[l] >= p[r]) {
      p[l] += p[r];
      recur(l, r - 1);
      ans += cur - x[l];
      cur = x[l];
      return;
    } else {
      p[r] += p[l];
      recur(l + 1, r);
      ans += x[r] - cur;
      cur = x[r];
    }
  }

  public long solve(int n, int s, long[] x, long[] p) {
    // Implement your solution here
    this.x = x;
    this.p = p;
    cur = s;

    recur(0, n - 1);
    if (x[0] <= s && s <= x[n - 1])
      ans += x[n - 1] - x[0];
    return this.ans;
  }
}

class Main {
  /**
   * @param n: Number of apartments
   * @param s: Initial location of the board
   * @param x: array of positions of apartments
   * @param p: array of number of employees in each apartment
   * @return: return the number of seconds the bus
   */

  public static long solve(int n, int s, long[] x, long[] p) {
    // Implement your solution here

    return -1;
  }

  public static void main(String[] args) throws java.lang.Exception {
    Scanner input = new Scanner(System.in);

    int n = input.nextInt();
    int s = input.nextInt();
    long x[] = new long[n];
    long p[] = new long[n];

    for (int i = 0; i < n; i++) {
      x[i] = input.nextLong();
      p[i] = input.nextLong();
    }
    Solution sol = new Solution();
    System.out.println(sol.solve(n, s, x, p));
    input.close();
  }
}
```
{% endtab %}

{% tab Code Python %}
```python
import sys

# If you want to use a deep recursion, you need to increase the recursion limit
# Python's default recursion limit is 1000, so we need to increase it
sys.setrecursionlimit(10**8)


class Solution:
    @staticmethod
    def solve_range(
        s: int, x: list[int], p: list[int], l: int, r: int
    ) -> tuple[int, int]:
        """
        Args:
            s: Current position of the bus
            x: List of apartment positions on the number line
            p: List of number of employees in each apartment
            l: Left boundary index of the current range of apartments
            r: Right boundary index of the current range of apartments

        Returns:
            tuple[int, int]: A tuple containing:
                - The final position of the bus
                - The total time (distance traveled) required
        """
        if s < x[l]:
            return x[r], x[r] - s
        elif s > x[r]:
            return x[l], s - x[l]
        elif p[l] >= p[r]:
            p[l] += p[r]
            s, ans = Solution.solve_range(s, x, p, l, r - 1)
            return x[r], ans + x[r] - s
        else:
            p[r] += p[l]
            s, ans = Solution.solve_range(s, x, p, l + 1, r)
            return x[l], ans + s - x[l]

    @staticmethod
    def solve(n: int, s: int, x: list[int], p: list[int]) -> int:
        """
        Args:
            n: Number of apartments
            s: Initial position of the bus (NTU office location)
            x: List of apartment positions on the number line
            p: List of number of employees in each apartment

        Returns:
            int: The total time (in seconds) required for all employees to reach their apartments
        """
        return Solution.solve_range(s, x, p, 0, n - 1)[1]


if __name__ == "__main__":
    n, s = map(int, input().split())
    x, p = map(list, zip(*[map(int, input().split()) for _ in range(n)]))
    print(Solution.solve(n, s, x, p))
```
{% endtab %}
{% endtabs %}
