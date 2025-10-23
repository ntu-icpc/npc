---
title: C - Sodoku
layout: page
parent: NPC25 The 4th Contest Solution
---

# C - Sodoku

## Subtask 1
{% tabs FourthC %}
{% tab FourthC Python %}
```python
def has_unique_digits(n: int) -> bool:
    """
    Checks if a number has unique digits.
    """
    s = str(n)
    return len(s) == len(set(s))


def main():
    """
    Main function to read input and find the smallest Sodoku number.
    """
    try:
        n = int(input())
        while not has_unique_digits(n):
            n += 1
        print(n)
    except (EOFError, ValueError):
        # Handle cases with no input or invalid input
        pass


if __name__ == "__main__":
    main()
```
{% endtab %}
{% tab FourthC C %}
```c
#include <stdbool.h>
#include <stdio.h>

// Function to check if a number has unique digits
bool hasUniqueDigits(long long n) {
  long long d = 0;
  long long x = n;

  // Handle the case of 0 separately if it can be a single-digit input
  if (x == 0) {
    return true;
  }

  while (x > 0) {
    int digit = x % 10;
    if (d & (1 << digit)) {
      return false;  // Digit is repeated
    }
    d |= (1LL << digit);
    x /= 10;
  }
  return true;  // All digits are unique
}

int main() {
  long long n;
  scanf("%lld", &n);

  while (true) {
    if (hasUniqueDigits(n)) {
      break;
    }
    n++;
  }

  printf("%lld\n", n);
  return 0;
}

```
{% endtab %}
{% tab FourthC Cpp %}
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  long long n;
  cin >> n;

  while (true) {
    long long d = 0;

    long long x = n;
    bool can = true;
    while (x) {
      int digit = x % 10;
      if (d & (1LL << digit)) {
        can = false;
        break;
      }
      d |= (1LL << digit);
      x /= 10;
    }
    if (can) break;
    n++;
  }
  cout << n << "\n";
}

```
{% endtab %}
{% tab FourthC Java %}
```java
import java.util.Scanner;

class Solution_s1 {
  /**
   * Checks if a number has unique digits.
   * @param n The number to check.
   * @return true if all digits are unique, false otherwise.
   */
  public static boolean hasUniqueDigits(long n) {
    long d = 0;
    long x = n;

    // Handle the case of 0 separately
    if (x == 0) {
      return true;
    }

    while (x > 0) {
      int digit = (int) (x % 10);
      if ((d & (1 << digit)) != 0) {
        return false; // Digit is repeated
      }
      d |= (1 << digit);
      x /= 10;
    }
    return true; // All digits are unique
  }

  public static void main(String[] args) {
    Scanner input = new Scanner(System.in);
    long n = input.nextLong();
    input.close();

    while (true) {
      if (hasUniqueDigits(n)) {
        break;
      }
      n++;
    }
    System.out.println(n);
  }
}

```
{% endtab %}
{% endtabs %}

## Solution 

{% tabs FourthC %}
{% tab FourthC Python %}
```python
# If you want to use a deep recursion, you need to increase the recursion limit
# Python's default recursion limit is 1000
# Uncomment the following two lines if you need to use deep recursion
# import sys
# sys.setrecursionlimit(10**8)

from itertools import permutations


class Solution:
    @staticmethod
    def solve(n: int) -> int:
        """
        Args:
            n (int): The number given
        Returns:
            int: The lowest Sodoku number at least n
        """
        ans = 9876543210
        for perm in permutations(range(10)):
            num = 0
            for x in perm:
                num = num * 10 + x
                if num >= n:
                    ans = min(ans, num)
        return ans


if __name__ == "__main__":
    n = int(input())

    ans = Solution.solve(n)
    print(ans)

```
{% endtab %}
{% tab FourthC C %}
```c
#include <stdio.h>
#include <stdlib.h>

// Global variables to store the state during recursion
long long minSudokuNum;
long long targetN;

// Recursive function to find the smallest Sodoku number
void find(long long currentNum, int usedMask) {
  // If the number we've built is >= n, it's a potential answer.
  // Update our minimum answer.
  if (currentNum >= targetN) {
    if (currentNum < minSudokuNum) {
      minSudokuNum = currentNum;
    }
  }

  // Optimization: If currentNum is already greater than the best answer found,
  // any number built from it will also be greater. We can prune this branch.
  if (currentNum > minSudokuNum) {
    return;
  }

  // If all 10 digits have been used, we can't extend the number further.
  if (usedMask == (1 << 10) - 1) {
    return;
  }

  // Try appending each unused digit.
  for (int i = 0; i < 10; i++) {
    if ((usedMask & (1 << i)) == 0) {  // If digit i is not used
      long long nextNum = currentNum * 10 + i;
      find(nextNum, usedMask | (1 << i));
    }
  }
}

/**
 * @param n: the number given
 * @return: the lowest Sodoku number at least n
 */
long long solve(long long n) {
  // Initialize with the largest possible Sodoku number.
  minSudokuNum = 9876543210LL;
  targetN = n;

  // Start the recursive search.
  find(0, 0);

  return minSudokuNum;
}

int main() {
  long long n;
  scanf("%lld", &n);
  long long ans = solve(n);

  printf("%lld\n", ans);
  return 0;
}

```
{% endtab %}
{% tab FourthC C++ %}
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  set<long long> ans;

  vector<int> h = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
  do {
    long long n = 0;
    for (int i = 0; i < 10; i++) {
      n = n * 10 + h[i];
      ans.emplace(n);
    }
  } while (next_permutation(h.begin(), h.end()));

  sort(ans.begin(), ans.end());

  long long n;
  cin >> n;
  cout << *lower_bound(ans.begin(), ans.end(), n);
}

```
{% endtab %}
{% tab FourthC Java %}
```java
import java.util.Scanner;

class Solution {
  private static long minSudokuNum;
  private static long targetN;

  private static void find(long currentNum, int usedMask) {
    // If the number we've built is >= n, it's a potential answer.
    // Update our minimum answer.
    if (currentNum >= targetN) {
      minSudokuNum = Math.min(minSudokuNum, currentNum);
    }

    // Optimization: If currentNum is already greater than the best answer found,
    // any number built from it will also be greater. We can prune this branch.
    if (currentNum > minSudokuNum) {
      return;
    }

    // If all 10 digits have been used, we can't extend the number further.
    if (usedMask == (1 << 10) - 1) {
      return;
    }

    // Try appending each unused digit.
    for (int i = 0; i < 10; i++) {
      if ((usedMask & (1 << i)) == 0) { // If digit i is not used
        long nextNum = currentNum * 10 + i;
        find(nextNum, usedMask | (1 << i));
      }
    }
  }

  /**
   * @param n: the number given
   * @return: the lowest Sodoku number at least n
   */
  static long solve(long n) {
    // Initialize with the largest possible Sodoku number.
    minSudokuNum = 9876543210L;
    targetN = n;

    // Start the recursive search.
    // This mimics the Python generator starting with a state of (0, 0),
    // which explores 0, 1, 2... and also 01, 02, 10, 12 etc.
    find(0, 0);

    return minSudokuNum;
  }

  public static void main(String[] args) throws java.lang.Exception {
    Scanner input = new Scanner(System.in);

    long n = input.nextLong();
    System.out.println(solve(n));

    input.close();
  }
}

```
{% endtab %}
{% endtabs %}
