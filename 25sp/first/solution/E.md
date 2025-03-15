---
title: E - Operations
layout: page
parent: Solutions
---

# E - Operations

## Codes

{% tabs Code %}
{% tab Code C %}
```c
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>

/**
 * Solves the problem of finding the maximum achievable score.
 *
 * @param T The target score
 * @param A First increment value
 * @param B Second increment value
 * @return The maximum achievable score
 */
int solve(int T, int A, int B) {
  bool *f = calloc(T + 1, sizeof(bool));
  f[0] = true;

  for (int i = 0; i <= T; ++i) {
    if (f[i]) {
      if (i + A <= T) {
        f[i + A] = true;
      }
      if (i + B <= T) {
        f[i + B] = true;
      }
    }
  }

  for (int i = 0; i <= T; ++i) {
    if (f[i]) {
      f[i / 2] = true;
    }
  }

  for (int i = 0; i <= T; ++i) {
    if (f[i]) {
      if (i + A <= T) {
        f[i + A] = true;
      }
      if (i + B <= T) {
        f[i + B] = true;
      }
    }
  }

  for (int i = T; i >= 0; --i) {
    if (f[i]) {
      free(f);
      return i;
    }
  }

  free(f);
  return 0;
}

int main() {
  int T, A, B;
  scanf("%d %d %d", &T, &A, &B);
  printf("%d\n", solve(T, A, B));
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
   * @param n: the number of positions
   * @param k: the number of chosen positions
   * @param x: vector of positions
   * @return: return the maximum minimum distance between any two chosen
   * positions
   */
  static int solve(int T, int A, int B) {
    std::vector<bool> dp(T + 1);
    dp[0] = true;
    for (int i = 0; i <= T; ++i) {
      if (dp[i]) {
        if (i + A <= T) dp[i + A] = true;
        if (i + B <= T) dp[i + B] = true;
      }
    }
    for (int i = 0; i <= T; ++i)
      if (dp[i]) dp[i >> 1] = true;
    for (int i = 0; i <= T; ++i) {
      if (dp[i]) {
        if (i + A <= T) dp[i + A] = true;
        if (i + B <= T) dp[i + B] = true;
      }
    }
    for (int i = T; i; --i) {
      if (dp[i]) return i;
    }
  }
};

int main() {
  int T, A, B;
  std::cin >> T >> A >> B;
  std::cout << Solution::solve(T, A, B) << std::endl;
  return 0;
}
```
{% endtab %}

{% tab Code Java %}
```java
import java.util.Arrays;
import java.util.Scanner;

public class Solution {
  /**
   * @param t: The limit of the total capacity
   * @param a: The first increment value
   * @param b: The second increment value
   * @return: return the maximum achievable score
   */
  public static int solve(int t, int a, int b) {
    boolean dp1[] = new boolean[t + 1];
    Arrays.fill(dp1, false);

    dp1[0] = true;
    for (int i = 0; i < t; i++) {
      if (dp1[i]) {
        if (i + a <= t)
          dp1[i + a] = true;
        if (i + b <= t)
          dp1[i + b] = true;
      }
    }

    boolean dp2[] = new boolean[t + 1];
    Arrays.fill(dp2, false);
    for (int i = 0; i <= t; i++) {
      if (dp1[i])
        dp2[i / 2] = true;
    }

    for (int i = 0; i < t; i++) {
      if (dp2[i]) {
        if (i + a <= t)
          dp2[i + a] = true;
        if (i + b <= t)
          dp2[i + b] = true;
      }
    }

    for (int i = t; i >= 0; i--) {
      if (dp1[i] || dp2[i])
        return i;
    }

    return 0;
  }

  public static void main(String[] args) {
    Scanner input = new Scanner(System.in);

    int t = input.nextInt();
    int a = input.nextInt();
    int b = input.nextInt();

    System.out.println(solve(t, a, b));
    input.close();
  }
}
```
{% endtab %}

{% tab Code Python %}
```python
class Solution:
    @staticmethod
    def solve(t: int, a: int, b: int) -> int:
        """
        Solves the problem of finding the maximum achievable score.

        Args:
            t (int): The limit of the total capacity
            a (int): The first increment value
            b (int): The second increment value

        Returns:
            int: The maximum achievable score
        """
        f = [False] * (t + 1)
        f[0] = True

        for i in range(t + 1):
            if f[i]:
                if i + a <= t:
                    f[i + a] = True
                if i + b <= t:
                    f[i + b] = True

        for i in range(t + 1):
            if f[i]:
                f[i // 2] = True

        for i in range(t + 1):
            if f[i]:
                if i + a <= t:
                    f[i + a] = True
                if i + b <= t:
                    f[i + b] = True

        for i in range(t, -1, -1):
            if f[i]:
                return i

        return 0


if __name__ == "__main__":
    t, a, b = map(int, input().split())
    print(Solution.solve(t, a, b))
```
{% endtab %}
{% endtabs %}
