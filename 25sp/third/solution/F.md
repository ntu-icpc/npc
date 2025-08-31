---
title: F - Distance
layout: page
parent: NPC25 Welcome AY25/26 Contest Solution
---

# F - Distance

## $\mathcal{O}(n^5)$ Solution

{% tabs ThirdFn5 %}
{% tab ThirdFn5 Python %}
```python
from typing import List

# If you want to use a deep recursion, you need to increase the recursion limit
# Python's default recursion limit is 1000
# Uncomment the following two lines if you need to use deep recursion
# import sys
# sys.setrecursionlimit(10**8)


class Solution:
    @staticmethod
    def solve(n: int, polygon: List[int]) -> int:
        """
        Args:
            n (int): number of vertices
            polygon (list[int]): an array consisting n integers where the i-th integer represents the integer written on i-th vertices

        Returns:
            int: an integer representing the maximum achievable score
        """
        f = [[0] * n for _ in range(n)]

        def score(i, j, k):
            return polygon[i] * polygon[j] * polygon[k]

        for l in range(n - 1, -1, -1):
            for r in range(l + 1, n):
                for i in range(l, r + 1):
                    for j in range(i + 1, r + 1):
                        for k in range(j + 1, r + 1):
                            left_score = f[l][i - 1] if i > l else 0
                            mid1_score = f[i + 1][j - 1] if i + 1 <= j - 1 else 0
                            mid2_score = f[j + 1][k - 1] if j + 1 <= k - 1 else 0
                            right_score = f[k + 1][r] if k < r else 0
                            current_score = score(i, j, k)

                            total_score = (
                                left_score
                                + mid1_score
                                + mid2_score
                                + right_score
                                + current_score
                            )
                            f[l][r] = max(f[l][r], total_score)

        return f[0][n - 1]


if __name__ == "__main__":
    n = int(input())
    polygon = list(map(int, input().split()))

    solution = Solution.solve(n, polygon)
    print(solution)
```
{% endtab %}
{% tab ThirdFn5 C %}
```c
#include <inttypes.h>
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>

int64_t max(int64_t a, int64_t b) { return a > b ? a : b; }

/**
 * @param n: number of vertices
 * @param polygon: an array consisting n integers where the i-th integer
 * represents the integer written on i-th vertices
 * @return: return a long integer representing the maximum achievable score
 */
int64_t solve(int n, int *polygon) {
  int64_t **f = (int64_t **)calloc(n, sizeof(int64_t *));
  for (int i = 0; i < n; ++i) {
    f[i] = (int64_t *)calloc(n, sizeof(int64_t));
  }

  for (int l = n - 1; l >= 0; --l) {
    for (int r = l + 1; r < n; ++r) {
      for (int i = l; i < r + 1; ++i) {
        for (int j = i + 1; j < r + 1; ++j) {
          for (int k = j + 1; k < r + 1; ++k) {
            int64_t left_score = (i > l) ? f[l][i - 1] : 0;
            int64_t mid1_score = (i + 1 <= j - 1) ? f[i + 1][j - 1] : 0;
            int64_t mid2_score = (j + 1 <= k - 1) ? f[j + 1][k - 1] : 0;
            int64_t right_score = (k < r) ? f[k + 1][r] : 0;
            int64_t current_score =
                (int64_t)polygon[i] * polygon[j] * polygon[k];
            int64_t total_score = left_score + mid1_score + mid2_score +
                                  right_score + current_score;
            f[l][r] = max(f[l][r], total_score);
          }
        }
      }
    }
  }

  int64_t ans = f[0][n - 1];

  for (int i = 0; i < n; ++i) {
    free(f[i]);
  }
  free(f);

  return ans;
}

int main() {
  int n;
  scanf("%d", &n);
  int *polygon = (int *)calloc(n, sizeof(int));
  for (int i = 0; i < n; ++i) {
    scanf("%d", &polygon[i]);
  }
  int64_t ans = solve(n, polygon);
  printf("%" PRId64 "\n", ans);
  free(polygon);
  return 0;
}
```
{% endtab %}
{% tab ThirdFn5 C++ %}
```cpp
#include <algorithm>
#include <cstdint>
#include <iostream>
#include <vector>

class Solution {
 public:
  /**
   * @param n: number of vertices
   * @param polygon: an array consisting n integers where the i-th integer
   * represents the integer written on i-th vertices
   * @return: return a long integer representing the maximum achievable score
   */
  static int64_t solve(int n, std::vector<int> polygon) {
    std::vector<std::vector<int64_t>> f(n, std::vector<int64_t>(n, 0));
    auto score = [&](int i, int j, int k) {
      return (int64_t)polygon[i] * polygon[j] * polygon[k];
    };

    for (int l = n - 1; l >= 0; --l) {
      for (int r = l + 1; r < n; ++r) {
        for (int i = l; i < r + 1; ++i) {
          for (int j = i + 1; j < r + 1; ++j) {
            for (int k = j + 1; k < r + 1; ++k) {
              int64_t left_score = (i > l) ? f[l][i - 1] : 0;
              int64_t mid1_score = (i + 1 <= j - 1) ? f[i + 1][j - 1] : 0;
              int64_t mid2_score = (j + 1 <= k - 1) ? f[j + 1][k - 1] : 0;
              int64_t right_score = (k < r) ? f[k + 1][r] : 0;
              int64_t current_score = score(i, j, k);
              int64_t total_score = left_score + mid1_score + mid2_score +
                                    right_score + current_score;
              f[l][r] = std::max(f[l][r], total_score);
            }
          }
        }
      }
    }
    return f[0][n - 1];
  }
};

int main() {
  int n;
  std::cin >> n;
  std::vector<int> polygon(n);
  for (int i = 0; i < n; ++i) {
    std::cin >> polygon[i];
  }
  int64_t ans = Solution::solve(n, polygon);
  std::cout << ans << "\n";
  return 0;
}
```
{% endtab %}
{% tab ThirdFn5 Java %}
```java
import java.lang.Math;
import java.util.Scanner;

class Solution_n5 {
  /**
   * @param n: number of vertices
   * @param polygon: an array consisting n integers where the i-th integer represents the integer
   *     written on i-th vertices
   * @return: return a long integer representing the maximum achievable score
   */
  static long solve(int n, int[] polygon) {
    long[][] f = new long[n][n];
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < n; j++) {
        f[i][j] = 0;
      }
    }

    for (int l = n - 1; l >= 0; l--) {
      for (int r = l + 1; r < n; r++) {
        for (int i = l; i < r + 1; i++) {
          for (int j = i + 1; j < r + 1; j++) {
            for (int k = j + 1; k < r + 1; k++) {
              long left_score = (i > l) ? f[l][i - 1] : 0;
              long mid1_score = (i + 1 <= j - 1) ? f[i + 1][j - 1] : 0;
              long mid2_score = (j + 1 <= k - 1) ? f[j + 1][k - 1] : 0;
              long right_score = (k < r) ? f[k + 1][r] : 0;
              long current_score = (long) polygon[i] * polygon[j] * polygon[k];
              long total_score = left_score + mid1_score + mid2_score + right_score + current_score;
              f[l][r] = Math.max(f[l][r], total_score);
            }
          }
        }
      }
    }
    return f[0][n - 1];
  }

  public static void main(String[] args) throws java.lang.Exception {
    Scanner input = new Scanner(System.in);

    int n = input.nextInt();

    int[] polygon = new int[n];
    for (int i = 0; i < n; i++) {
      polygon[i] = input.nextInt();
    }

    long ans = solve(n, polygon);
    System.out.println(ans);

    input.close();
  }
}
```
{% endtab %}
{% tab ThirdFn5 Go %}
```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
)

func solve(n int, polygon []int) int64 {
	f := make([][]int64, n)
	for i := range f {
		f[i] = make([]int64, n)
	}

	score := func(i, j, k int) int64 {
		return int64(polygon[i]) * int64(polygon[j]) * int64(polygon[k])
	}

	for l := n - 1; l >= 0; l-- {
		for r := l + 1; r < n; r++ {
			for i := l; i < r+1; i++ {
				for j := i + 1; j < r+1; j++ {
					for k := j + 1; k < r+1; k++ {
						var left_score int64 = 0
						if i > l {
							left_score = f[l][i-1]
						}
						var mid1_score int64 = 0
						if i+1 <= j-1 {
							mid1_score = f[i+1][j-1]
						}
						var mid2_score int64 = 0
						if j+1 <= k-1 {
							mid2_score = f[j+1][k-1]
						}
						var right_score int64 = 0
						if k < r {
							right_score = f[k+1][r]
						}
						current_score := score(i, j, k)
						total_score := left_score + mid1_score + mid2_score + right_score + current_score
						f[l][r] = max(f[l][r], total_score)
					}
				}
			}
		}
	}
	return f[0][n-1]
}

func main() {
	reader := bufio.NewReader(os.Stdin)
	line, _ := reader.ReadString('\n')
	line = strings.TrimSpace(line)
	n, _ := strconv.Atoi(line)

	polygon := make([]int, n)
	line, _ = reader.ReadString('\n')
	line = strings.TrimSpace(line)
	parts := strings.Split(line, " ")
	for i := 0; i < n; i++ {
		polygon[i], _ = strconv.Atoi(parts[i])
	}

	result := solve(n, polygon)
	fmt.Println(result)
}

func max(a, b int64) int64 {
	if a > b {
		return a
	}
	return b
}
```
{% endtab %}
{% endtabs %}

## $\mathcal{O}(n^4)$ Solution

{% tabs ThirdFn4 %}
{% tab ThirdFn4 Python %}
```python
from typing import List

# If you want to use a deep recursion, you need to increase the recursion limit
# Python's default recursion limit is 1000
# Uncomment the following two lines if you need to use deep recursion
# import sys
# sys.setrecursionlimit(10**8)


class Solution:
    @staticmethod
    def solve(n: int, polygon: List[int]) -> int:
        """
        Args:
            n (int): number of vertices
            polygon (list[int]): an array consisting n integers where the i-th integer represents the integer written on i-th vertices

        Returns:
            int: an integer representing the maximum achievable score
        """
        f = [[0] * (n + 1) for _ in range(n + 1)]
        score = lambda i, j, k: polygon[i] * polygon[j] * polygon[k]

        for l in range(n - 1, -1, -1):
            for r in range(l + 1, n):
                f[l][r] = f[l + 1][r]
                for k1 in range(l + 1, r):
                    for k2 in range(k1 + 1, r + 1):
                        f[l][r] = max(
                            f[l][r],
                            f[l + 1][k1 - 1] + f[k1 + 1][k2 - 1] + f[k2 + 1][r] + score(l, k1, k2)
                        )
        
        # import pprint
        # pprint.pprint(f)
        
        return f[0][n - 1]


if __name__ == "__main__":
    n = int(input())
    polygon = list(map(int, input().split()))

    solution = Solution.solve(n, polygon)
    print(solution)
```
{% endtab %}
{% tab ThirdFn4 C %}
```c
#include <stdio.h>
#include <stdlib.h>

long long max(long long a, long long b) {
    return a > b ? a : b;
}

long long solve(int n, int* polygon) {
    long long** f = (long long**)malloc((n + 1) * sizeof(long long*));
    for (int i = 0; i <= n; i++) {
        f[i] = (long long*)calloc(n + 1, sizeof(long long));
    }

    for (int l = n - 1; l >= 0; l--) {
        for (int r = l + 1; r < n; r++) {
            f[l][r] = f[l + 1][r];
            for (int k1 = l + 1; k1 < r; k1++) {
                for (int k2 = k1 + 1; k2 < n; k2++) { // Note: Python range r+1 is exclusive, C loop condition < n is similar
                    if (k2 > r) continue; // Ensure k2 is within the current segment [l, r]
                    long long score = (long long)polygon[l] * polygon[k1] * polygon[k2];
                    long long current_score = f[l + 1][k1 - 1] + f[k1 + 1][k2 - 1] + f[k2 + 1][r] + score;
                    f[l][r] = max(f[l][r], current_score);
                }
            }
        }
    }

    long long result = f[0][n - 1];

    for (int i = 0; i <= n; i++) {
        free(f[i]);
    }
    free(f);

    return result;
}

int main() {
    int n;
    scanf("%d", &n);

    int* polygon = (int*)malloc(n * sizeof(int));
    for (int i = 0; i < n; i++) {
        scanf("%d", &polygon[i]);
    }

    long long solution = solve(n, polygon);
    printf("%lld\n", solution);

    free(polygon);

    return 0;
}
```
{% endtab %}
{% tab ThirdFn4 C++ %}
```cpp
#include <iostream>
#include <vector>
#include <numeric>
#include <algorithm>

class Solution {
public:
    static long long solve(int n, const std::vector<int>& polygon) {
        std::vector<std::vector<long long>> f(n + 1, std::vector<long long>(n + 1, 0));

        auto score = [&](int i, int j, int k) {
            return static_cast<long long>(polygon[i]) * polygon[j] * polygon[k];
        };

        for (int l = n - 1; l >= 0; --l) {
            for (int r = l + 1; r < n; ++r) {
                f[l][r] = f[l + 1][r];
                for (int k1 = l + 1; k1 < r; ++k1) {
                    for (int k2 = k1 + 1; k2 <= r; ++k2) {
                        f[l][r] = std::max(
                            f[l][r],
                            f[l + 1][k1 - 1] + f[k1 + 1][k2 - 1] + f[k2 + 1][r] + score(l, k1, k2)
                        );
                    }
                }
            }
        }
        
        return f[0][n - 1];
    }
};

int main() {
    std::ios_base::sync_with_stdio(false);
    std::cin.tie(NULL);

    int n;
    std::cin >> n;

    std::vector<int> polygon(n);
    for (int i = 0; i < n; ++i) {
        std::cin >> polygon[i];
    }

    long long solution = Solution::solve(n, polygon);
    std::cout << solution << std::endl;

    return 0;
}
```
{% endtab %}
{% tab ThirdFn4 Java %}
```java
import java.util.Scanner;

public class Solution_n4_2 {

    public static long solve(int n, int[] polygon) {
        long[][] f = new long[n + 1][n + 1];

        for (int l = n - 1; l >= 0; l--) {
            for (int r = l + 1; r < n; r++) {
                f[l][r] = f[l + 1][r];
                for (int k1 = l + 1; k1 < r; k1++) {
                    for (int k2 = k1 + 1; k2 <= r; k2++) {
                        long score = (long) polygon[l] * polygon[k1] * polygon[k2];
                        f[l][r] = Math.max(
                            f[l][r],
                            f[l + 1][k1 - 1] + f[k1 + 1][k2 - 1] + f[k2 + 1][r] + score
                        );
                    }
                }
            }
        }
        
        return f[0][n - 1];
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        int n = scanner.nextInt();
        int[] polygon = new int[n];
        for (int i = 0; i < n; i++) {
            polygon[i] = scanner.nextInt();
        }
        
        long solution = solve(n, polygon);
        System.out.println(solution);
        
        scanner.close();
    }
}
```
{% endtab %}
{% tab ThirdFn4 Go %}
```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
)

func max(a, b int64) int64 {
	if a > b {
		return a
	}
	return b
}

func solve(n int, polygon []int) int64 {
	f := make([][]int64, n+1)
	for i := range f {
		f[i] = make([]int64, n+1)
	}

	score := func(i, j, k int) int64 {
		return int64(polygon[i]) * int64(polygon[j]) * int64(polygon[k])
	}

	for l := n - 1; l >= 0; l-- {
		for r := l + 1; r < n; r++ {
			f[l][r] = f[l+1][r]
			for k1 := l + 1; k1 < r; k1++ {
				for k2 := k1 + 1; k2 <= r; k2++ {
					f[l][r] = max(
						f[l][r],
						f[l+1][k1-1]+f[k1+1][k2-1]+f[k2+1][r]+score(l, k1, k2),
					)
				}
			}
		}
	}

	return f[0][n-1]
}

func main() {
	reader := bufio.NewReader(os.Stdin)

	line, _ := reader.ReadString('\n')
	line = strings.TrimSpace(line)
	n, _ := strconv.Atoi(line)

	line, _ = reader.ReadString('\n')
	line = strings.TrimSpace(line)
	parts := strings.Split(line, " ")
	polygon := make([]int, n)
	for i := 0; i < n; i++ {
		polygon[i], _ = strconv.Atoi(parts[i])
	}

	solution := solve(n, polygon)
	fmt.Println(solution)
}
```
{% endtab %}
{% endtabs %}

## $\mathcal{O}(n^3)$ Solution

{% tabs ThirdFn3 %}
{% tab ThirdFn3 Python %}
```python
from typing import List

# If you want to use a deep recursion, you need to increase the recursion limit
# Python's default recursion limit is 1000
# Uncomment the following two lines if you need to use deep recursion
# import sys
# sys.setrecursionlimit(10**8)


class Solution:
    @staticmethod
    def solve(n: int, polygon: List[int]) -> int:
        """
        Args:
            n (int): number of vertices
            polygon (list[int]): an array consisting n integers where the i-th integer represents the integer written on i-th vertices

        Returns:
            int: an integer representing the maximum achievable score
        """
        f = [[0] * n for _ in range(n)]
        score = lambda i, j, k: polygon[i] * polygon[j] * polygon[k]
        for l in range(n - 1, -1, -1):
            for r in range(l + 1, n):
                for m in range(l, r):
                    f[l][r] = max(f[l][r], f[l][m] + f[m + 1][r])
                    if m != l:
                        f[l][r] = max(
                            f[l][r], f[l + 1][m - 1] + f[m + 1][r - 1] + score(l, m, r)
                        )
        return f[0][n - 1]


if __name__ == "__main__":
    n = int(input())
    polygon = list(map(int, input().split()))

    solution = Solution.solve(n, polygon)
    print(solution)
```
{% endtab %}
{% tab ThirdFn3 C %}
```c
#include <inttypes.h>
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>

int64_t max(int64_t a, int64_t b) { return a > b ? a : b; }

/**
 * @param n: number of vertices
 * @param polygon: an array consisting n integers where the i-th integer
 * represents the integer written on i-th vertices
 * @return: return a long integer representing the maximum achievable score
 */
int64_t solve(int n, int *polygon) {
  int64_t **f = (int64_t **)calloc(n, sizeof(int64_t *));
  for (int i = 0; i < n; ++i) {
    f[i] = (int64_t *)calloc(n, sizeof(int64_t));
  }

  for (int l = n - 1; l >= 0; --l) {
    for (int r = l + 1; r < n; ++r) {
      for (int m = l; m < r; ++m) {
        f[l][r] = max(f[l][r], f[l][m] + f[m + 1][r]);
        if (m != l) {
          int64_t term1 = (l + 1 <= m - 1) ? f[l + 1][m - 1] : 0;
          int64_t term2 = (m + 1 <= r - 1) ? f[m + 1][r - 1] : 0;
          int64_t current_score = (int64_t)polygon[l] * polygon[m] * polygon[r];
          f[l][r] = max(f[l][r], term1 + term2 + current_score);
        }
      }
    }
  }

  int64_t ans = f[0][n - 1];

  for (int i = 0; i < n; ++i) {
    free(f[i]);
  }
  free(f);

  return ans;
}

int main() {
  int n;
  scanf("%d", &n);
  int *polygon = (int *)calloc(n, sizeof(int));
  for (int i = 0; i < n; ++i) {
    scanf("%d", &polygon[i]);
  }
  int64_t ans = solve(n, polygon);
  printf("%" PRId64 "\n", ans);
  free(polygon);
  return 0;
}
```
{% endtab %}
{% tab ThirdFn3 C++ %}
```cpp
#include <algorithm>
#include <cstdint>
#include <iostream>
#include <vector>

class Solution {
 public:
  /**
   * @param n: number of vertices
   * @param polygon: an array consisting n integers where the i-th integer
   * represents the integer written on i-th vertices
   * @return: return a long integer representing the maximum achievable score
   */
  static int64_t solve(int n, std::vector<int> polygon) {
    std::vector<std::vector<int64_t>> f(n, std::vector<int64_t>(n, 0));
    auto score = [&](int i, int j, int k) {
      return (int64_t)polygon[i] * polygon[j] * polygon[k];
    };

    for (int l = n - 1; l >= 0; --l) {
      for (int r = l + 1; r < n; ++r) {
        for (int m = l; m < r; ++m) {
          f[l][r] = std::max(f[l][r], f[l][m] + f[m + 1][r]);
          if (m != l) {
            int64_t term1 = (l + 1 <= m - 1) ? f[l + 1][m - 1] : 0;
            int64_t term2 = (m + 1 <= r - 1) ? f[m + 1][r - 1] : 0;
            f[l][r] = std::max(f[l][r], term1 + term2 + score(l, m, r));
          }
        }
      }
    }
    return f[0][n - 1];
  }
};

int main() {
  int n;
  std::cin >> n;
  std::vector<int> polygon(n);
  for (int i = 0; i < n; ++i) {
    std::cin >> polygon[i];
  }
  int64_t ans = Solution::solve(n, polygon);
  std::cout << ans << "\n";
  return 0;
}
```
{% endtab %}
{% tab ThirdFn3 Java %}
```java
import java.lang.Math;
import java.util.Scanner;

class Solution {
  /**
   * @param n: number of vertices
   * @param polygon: an array consisting n integers where the i-th integer represents the integer
   *     written on i-th vertices
   * @return: return a long integer representing the maximum achievable score
   */
  static long solve(int n, int[] polygon) {
    long[][] f = new long[n][n];
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < n; j++) {
        f[i][j] = 0;
      }
    }

    for (int l = n - 1; l >= 0; l--) {
      for (int r = l + 1; r < n; r++) {
        for (int m = l; m < r; m++) {
          f[l][r] = Math.max(f[l][r], f[l][m] + f[m + 1][r]);
          if (m != l) {
            long term1 = (l + 1 <= m - 1) ? f[l + 1][m - 1] : 0;
            long term2 = (m + 1 <= r - 1) ? f[m + 1][r - 1] : 0;
            long current_score = (long) polygon[l] * polygon[m] * polygon[r];
            f[l][r] = Math.max(f[l][r], term1 + term2 + current_score);
          }
        }
      }
    }
    return f[0][n - 1];
  }

  public static void main(String[] args) throws java.lang.Exception {
    Scanner input = new Scanner(System.in);

    int n = input.nextInt();

    int[] polygon = new int[n];
    for (int i = 0; i < n; i++) {
      polygon[i] = input.nextInt();
    }

    long ans = solve(n, polygon);
    System.out.println(ans);

    input.close();
  }
}
```
{% endtab %}
{% tab ThirdFn3 Go %}
```go
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
)

func solve(n int, polygon []int) int64 {
	f := make([][]int64, n)
	for i := range f {
		f[i] = make([]int64, n)
	}

	score := func(i, j, k int) int64 {
		return int64(polygon[i]) * int64(polygon[j]) * int64(polygon[k])
	}

	for l := n - 1; l >= 0; l-- {
		for r := l + 1; r < n; r++ {
			for m := l; m < r; m++ {
				f[l][r] = max(f[l][r], f[l][m]+f[m+1][r])
				if m != l {
					var term1 int64 = 0
					if l+1 <= m-1 {
						term1 = f[l+1][m-1]
					}
					var term2 int64 = 0
					if m+1 <= r-1 {
						term2 = f[m+1][r-1]
					}
					f[l][r] = max(f[l][r], term1+term2+score(l, m, r))
				}
			}
		}
	}
	return f[0][n-1]
}

func main() {
	reader := bufio.NewReader(os.Stdin)
	line, _ := reader.ReadString('\n')
	line = strings.TrimSpace(line)
	n, _ := strconv.Atoi(line)

	polygon := make([]int, n)
	line, _ = reader.ReadString('\n')
	line = strings.TrimSpace(line)
	parts := strings.Split(line, " ")
	for i := 0; i < n; i++ {
		polygon[i], _ = strconv.Atoi(parts[i])
	}

	result := solve(n, polygon)
	fmt.Println(result)
}

func max(a, b int64) int64 {
	if a > b {
		return a
	}
	return b
}
```
{% endtab %}
{% endtabs %}
