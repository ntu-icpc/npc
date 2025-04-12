---
title: D - Game
layout: page
parent: Solutions
---

# D - Game

## Abridged Problem Statement

Given an array that consists of integers. Use the set of cards to move to get the maximal sum.

Subtasks:

- For all task, $$1 \leq N \leq 10^5, 1\leq R \leq 10^9, 0 \leq \vert a_i\vert \leq 10^9$$
- Subtask 1 & 2: $$K = 1$$
- Subtask 3: $$K = 2$$
- Subtask 4: $$K = 4$$
- Subtask 5: $$K = 8$$

## Observations

{: .note-title }
> Observation 1
> 
> The total distance moved is fixed until the set of movement cards is flushed.

{: .note-title }
> Observation 2
> 
> Always use the energy card as soon as possible.

## Subtask 1 & 2

The movement sequence is fixed. Just use the following strategy:

- Use the energy card.
- Use the movement card, check whether the current energy is negative.
- If the destination is not reached, the cards are replenished. Repeat this strategy.

{: .note-title }
> Motivation
>
> These subtasks are designed for contestants to verify their understanding of the problem statement, especially about **"whether an energy card could be used when we have already arrived at the destination"**.

## Subtask 3

$$K = 2$$ (There are only two movement cards), if we use the energy card first, then the only two possible strategies are as follows:

- Use the first movement card, then the second movement card.
- Use the second movement card, then the first movement card.

It is easy to compare which is better (by implementing both strategies and taking the better one). Just be careful about **if there is a sudden death situation**.

## Subtask 4

$$K=4$$.

Based on the two observations and the methodology we used in subtask 3, we can have the following basic idea.

{: .note-title }
> Key Conclusion
>
> If we already stand on the pillar at position $$x$$, we do not care what happened before that. We only want to have higher energy at the current time.

Based on the conclusion, let's start with position 0.

Firstly, we use the energy card and get $$Energy = R$$.

Then, we perform a complete search over all possible orders of the movement cards used.

Our goal is to find the best order which can provide us the highest $$Energy$$ without sudden death.

The complexity in this step is $$K \cdot K!$$ (There are $$K!$$ possible permutations of movement cards, and we perform an iteration over the movement cards for each possible permutation).

Whatever the best order is, we will always stand at $$\sum_i^k b_i$$ th pillar.

And we hold the highest possible $$Energy$$ now (through complete search).

Now our deck of cards is flushed. In the next round, our goal is similar to the first round.

Our goal is to find the best order which can provide us the highest $$\Delta Energy$$ without sudden death.

The complexity in this step is $$K \cdot K!$$.

This kind of consideration is called **Optimal substructure**.

Just repeat this complete search until she arrives at the destination.

{: .note-title }
> Time Complexity
>
> In each round, we need a complete search, and we have $$N/\sum b_i$$ rounds.
>
> The Time Complexity is $$O\left(\frac{N}{\sum b_i} K\cdot K!\right)$$, which cannot pass subtask 5.
>
> **But if you can solve the subtask, it is already a huge success!**

{% tabs Subtask4 %}
{% tab Subtask4 Python %}
```python
class Solution:
    @staticmethod
    def get_permutations(n: int) -> list[list[int]]:
        if n == 1:
            return [[1]]
        else:
            result_prev = Solution.get_permutations(n - 1)
            answer = [[n] + p for p in result_prev]
            for i in range(1, n):
                answer.extend([p[:i] + [n] + p[i:] for p in result_prev])
            return answer

    @staticmethod
    def solve(N: int, K: int, R: int, A: list[int], B: list[int]) -> int:
        f = [-1] * (N + 1)
        ans = -1
        f[0] = 0
        permutations = Solution.get_permutations(K)
        for i in range(0, N + 1):
            if f[i] < 0:
                continue
            for p in permutations:
                current_energy = f[i] + R
                current_pos = i
                for j in range(K):
                    current_pos += B[p[j] - 1]
                    if current_pos > N:
                        ans = max(ans, current_energy)
                        break
                    else:
                        current_energy += A[current_pos - 1]
                        if current_energy < 0:
                            break
                if current_energy >= 0 and current_pos <= N:
                    f[current_pos] = max(f[current_pos], current_energy)
        return ans


if __name__ == "__main__":
    N, K, R = map(int, input().split())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))
    print(Solution.solve(N, K, R, A, B))
```
{% endtab %}
{% tab Subtask4 C++ %}
```cpp
#include <algorithm>
#include <iostream>
#include <vector>

class Solution {
 public:
  /**
   * @param n The total number of pillars
   * @param k The number of movement cards
   * @param r The energy of the energy card
   * @param a vector of pillar energy fluctuations [1-index]
   * @param b vector of the teleport distance for movement card [1-index]
   * @return  The maximum possible energy, -1 if impossible
   */
  static long long solve(int n, int k, int r, std::vector<int>& a,
                         std::vector<int>& b) {
    long long ans = 0;
    int pos = 0;
    std::vector<int> t(k);
    for (int i = 0; i < k; ++i) t[i] = i;
    int step = 0;
    for (int i = 0; i < k; ++i) step += b[i + 1];
    while (pos <= n) {
      long long maxDelta = -1e17;
      do {
        long long delta = r;
        int i = 0;
        int next = pos + b[t[0] + 1];
        ++i;
        bool impossible = false;
        while (next <= n && i < k) {
          delta += a[next];
          next += b[t[i] + 1];
          ++i;
          if (ans + delta < 0) {
            impossible = true;
            break;
          }
        }
        if (impossible) {
          continue;
        }
        if (next <= n) delta += a[next];

        maxDelta = std::max(maxDelta, delta);

      } while (std::next_permutation(t.begin(), t.end()));
      pos += step;
      if (maxDelta == -1e17) {
        return -1;
      }
      ans += maxDelta;
      if (ans < 0) return -1;
    }
    if (ans < 0) return -1;
    return ans;
  }
};

int main() {
  int n, k, r;
  std::cin >> n >> k >> r;
  std::vector<int> a(n + 1), b(k + 1);
  for (int i = 1; i <= n; ++i) std::cin >> a[i];
  for (int i = 1; i <= k; ++i) std::cin >> b[i];
  long long ans = Solution::solve(n, k, r, a, b);
  std::cout << ans << std::endl;
}
```
{% endtab %}
{% endtabs %}


## Subtask 5

We would like to introduce a type of dynamic programming problem here called **Bitmask DP**.

The dynamic programming state is $$DP[i][bitmask]$$ representing the best $$Energy$$ we could have when we stand on the $$i$$ th pillar and the movement cards used are represented by $$bitmask$$.

- we stand at $$i$$-th pillar
- The remaining cards we hold in our hand are encoded by the bitmask (which is explained below).
- bitmask is an integer (we understood it in binary representation), where the $$j$$th bit from Least Significant Bit is on ($$=1$) implies that we still have the $$j$$th card on hand.

### Explaination

For example $$DP[23][11]$$. We have $$11_{10} = 1011_{2}$$.

The value of $$DP[23][11]$$ is the maximal possible energy he can get when he stands in the 23rd Pillar and holds the first, second and the fourth cards.

### State Transition

$$DP[0][11111111_{2}]=R$$ (used the energy card)

$$\rightarrow$$ $$DP[b[1]][11111110_2] = max(itself, DP[0][11111111_2] + a[b[1]]$$

$$\rightarrow$$ $$DP[b[2]][11111101_2] = max(itself, DP[0][11111111_2] + a[b[2]]$$

...

More generally, for $$DP[i][j]$$ we can try to use each remaining card - condition:($$j \& 2^{k-1}\neq0$$)

$$DP[i+b[k]][j\oplus2^{k-1}] = max(itself, DP[i][j] + a[i+b[k]])$$

- Special case 1:If $$j\oplus 2^{k-1} =0$$, we flush the set of cards and use the energy card immediately.

- Special case 2:If $$i+b[k] >N$$, we reached and destination.

### Time & Space Complexity

The size of the state space is $$N*2^K$$ and each state have $$\mathtt{bitcount}(K)$$ transitions.

- Space Complexity: $$O(N*2^K)$$
- Time Complexity: $$O(NK*2^K)$$

### Implementation

{% tabs Subtask5 %}
{% tab Subtask5 Python %}
```python
class Solution:
    @staticmethod
    def solve(N: int, K: int, R: int, A: list[int], B: list[int]) -> int:
        f = [[-1] * (1 << K) for _ in range(N + 2)]
        f[0][0] = R
        A.append(0)
        for i in range(0, N + 1):
            for j in range(1 << K):
                curr_energy = f[i][j]
                if curr_energy < 0:
                    continue
                for k in range(K):
                    if j & (1 << k) != 0:
                        continue
                    next_pos = min(i + B[k], N + 1)
                    next_energy = curr_energy + A[next_pos - 1]
                    if next_energy < 0:
                        continue
                    next_state = j | (1 << k)
                    if next_state == (1 << K) - 1 and next_pos <= N:
                        next_state = 0
                        next_energy += R
                    f[next_pos][next_state] = max(f[next_pos][next_state], next_energy)
        return max(f[N + 1])


if __name__ == "__main__":
    N, K, R = map(int, input().split())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))
    print(Solution.solve(N, K, R, A, B))
```
{% endtab %}
{% tab Subtask5 C %}
```c
#include <inttypes.h>
#include <stdint.h>
#include <stdio.h>

int64_t solve(int n, int k, int r, int *a, int *b);

int main() {
  int n, k, r;
  scanf("%d %d %d", &n, &k, &r);
  int *a = (int *)malloc(n * sizeof(int));
  int *b = (int *)malloc(k * sizeof(int));
  for (int i = 0; i < n; i++) {
    scanf("%d", &a[i]);
  }
  for (int i = 0; i < k; i++) {
    scanf("%d", &b[i]);
  }
  int64_t ans = solve(n, k, r, a, b);
  printf("%" PRId64 "\n", ans);
  free(a);
  free(b);
  return 0;
}

int64_t solve(int n, int k, int r, int *a, int *b) {
  // Create a 2D array for dynamic programming
  int64_t **f = (int64_t **)malloc((n + 2) * sizeof(int64_t *));
  for (int i = 0; i <= n + 1; i++) {
    f[i] = (int64_t *)malloc((1 << k) * sizeof(int64_t));
    for (int j = 0; j < (1 << k); j++) {
      f[i][j] = -1;
    }
  }

  // Initial state: position 0 with no cards used and energy r
  f[0][0] = r;

  // Create an extended array with an extra element
  int *extended_a = (int *)malloc((n + 1) * sizeof(int));
  for (int i = 0; i < n; i++) {
    extended_a[i] = a[i];
  }
  extended_a[n] = 0;

  // Dynamic programming
  for (int i = 0; i <= n; i++) {
    for (int j = 0; j < (1 << k); j++) {
      int64_t currEnergy = f[i][j];
      if (currEnergy < 0) {
        continue;
      }

      for (int t = 0; t < k; t++) {
        if (j & (1 << t)) {
          continue;
        }

        int nextPos = i + b[t];
        if (nextPos > n + 1) {
          nextPos = n + 1;
        }

        int64_t nextEnergy = currEnergy + extended_a[nextPos - 1];

        if (nextEnergy < 0) {
          continue;
        }

        int nextState = j | (1 << t);
        if (nextState == (1 << k) - 1 && nextPos <= n) {
          nextState = 0;
          nextEnergy += r;
        }

        if (f[nextPos][nextState] < nextEnergy) {
          f[nextPos][nextState] = nextEnergy;
        }
      }
    }
  }

  // Find the maximum energy at the destination
  int64_t ans = -1;
  for (int i = 0; i < (1 << k); i++) {
    if (f[n + 1][i] > ans) {
      ans = f[n + 1][i];
    }
  }

  // Free allocated memory
  for (int i = 0; i <= n + 1; i++) {
    free(f[i]);
  }
  free(f);
  free(extended_a);

  return ans;
}
```
{% endtab %}
{% tab Subtask5 C++ %}
```cpp
#include <algorithm>
#include <cstdint>
#include <iostream>
#include <vector>

class Solution {
 public:
  /**
   * @param n The total number of pillars
   * @param k The number of movement cards
   * @param r The energy of the energy card
   * @param a vector of pillar energy fluctuations
   * @param b vector of the teleport distance for movement card
   * @return  The maximum possible energy, -1 if impossible
   */
  static int64_t solve(int n, int k, int r, std::vector<int>& a,
                       std::vector<int>& b) {
    std::vector<std::vector<int64_t>> f(n + 2,
                                        std::vector<int64_t>(1 << k, -1));
    f[0][0] = r;
    a.push_back(0);

    for (int i = 0; i <= n; i++) {
      for (int j = 0; j < (1 << k); j++) {
        int64_t currEnergy = f[i][j];
        if (currEnergy < 0) {
          continue;
        }

        for (int t = 0; t < k; t++) {
          if (j & (1 << t)) {
            continue;
          }

          int nextPos = std::min(i + b[t], n + 1);
          int64_t nextEnergy = currEnergy + a[nextPos - 1];

          if (nextEnergy < 0) {
            continue;
          }

          int nextState = j | (1 << t);
          if (nextState == (1 << k) - 1 && nextPos <= n) {
            nextState = 0;
            nextEnergy += r;
          }

          f[nextPos][nextState] = std::max(f[nextPos][nextState], nextEnergy);
        }
      }
    }

    int64_t ans = -1;
    for (int i = 0; i < (1 << k); i++) {
      ans = std::max(ans, f[n + 1][i]);
    }

    return ans;
  }
};

int main() {
  int n, k, r;
  std::cin >> n >> k >> r;
  std::vector<int> a(n), b(k);
  for (int i = 0; i < n; ++i) {
    std::cin >> a[i];
  }
  for (int i = 0; i < k; ++i) {
    std::cin >> b[i];
  }
  int64_t ans = Solution::solve(n, k, r, a, b);
  std::cout << ans << std::endl;
  return 0;
}
```
{% endtab %}
{% tab Subtask5 Java %}
```java
import java.util.Scanner;

class Solution {
  /**
   * @param n: number of elements
   * @param k: size of the subsequence
   * @param a: array of integers
   * @return: return the minimum difference between max and min elements in any
   * subsequence of size k
   */
  static long solve(int n, int k, int r, int[] a, int[] b) {
    long[][] f = new long[n + 2][1 << k];
    for (int i = 0; i < n + 2; i++) {
      for (int j = 0; j < 1 << k; j++) {
        f[i][j] = -1;
      }
    }
    f[0][0] = r;

    int[] extendedA = new int[n + 1];
    System.arraycopy(a, 0, extendedA, 0, n);
    extendedA[n] = 0;

    for (int i = 0; i <= n; i++) {
      for (int j = 0; j < 1 << k; j++) {
        long currEnergy = f[i][j];
        if (currEnergy < 0) {
          continue;
        }
        for (int t = 0; t < k; t++) {
          if ((j & (1 << t)) != 0) {
            continue;
          }
          int nextPos = Math.min(i + b[t], n + 1);
          long nextEnergy = currEnergy + extendedA[nextPos - 1];
          if (nextEnergy < 0) {
            continue;
          }
          int nextState = j | (1 << t);
          if (nextState == (1 << k) - 1 && nextPos <= n) {
            nextState = 0;
            nextEnergy += r;
          }
          f[nextPos][nextState] = Math.max(f[nextPos][nextState], nextEnergy);
        }
      }
    }

    long ans = -1;
    for (int i = 0; i < 1 << k; i++) {
      ans = Math.max(ans, f[n + 1][i]);
    }
    return ans;
  }

  public static void main(String[] args) {
    Scanner input = new Scanner(System.in);

    int n = input.nextInt();
    int k = input.nextInt();
    int r = input.nextInt();
    int[] a = new int[n];
    int[] b = new int[k];
    for (int i = 0; i < n; i++) {
      a[i] = input.nextInt();
    }

    for (int i = 0; i < k; i++) {
      b[i] = input.nextInt();
    }

    System.out.println(solve(n, k, r, a, b));
    input.close();
  }
}
```
{% endtab %}
{% tab Subtask5 Go %}
```go
package main

import (
	"fmt"
)

func solve(n int, k int, r int, a []int, b []int) int {
	f := make([][]int, n+2)
	for i := 0; i < n+2; i++ {
		f[i] = make([]int, 1<<k)
		for j := 0; j < 1<<k; j++ {
			f[i][j] = -1
		}
	}
	f[0][0] = r
	a = append(a, 0)
	for i := 0; i <= n; i++ {
		for j := 0; j < 1<<k; j++ {
			currEnergy := f[i][j]
			if currEnergy < 0 {
				continue
			}
			for t := 0; t < k; t++ {
				if j&(1<<t) != 0 {
					continue
				}
				nextPos := min(i+b[t], n+1)
				nextEnergy := currEnergy + a[nextPos-1]
				if nextEnergy < 0 {
					continue
				}
				nextState := j | (1 << t)
				if nextState == (1<<k)-1 && nextPos <= n {
					nextState = 0
					nextEnergy += r
				}
				f[nextPos][nextState] = max(f[nextPos][nextState], nextEnergy)
			}
		}
	}
	ans := -1
	for i := 0; i < 1<<k; i++ {
		ans = max(ans, f[n+1][i])
	}
	return ans
}

func main() {
	var n, k, r int
	fmt.Scan(&n, &k, &r)

	var a, b []int
	a = make([]int, n)
	b = make([]int, k)
	for i := 0; i < n; i++ {
		fmt.Scan(&a[i])
	}
	for i := 0; i < k; i++ {
		fmt.Scan(&b[i])
	}

	fmt.Println(solve(n, k, r, a, b))
}
```
{% endtab %}
{% endtabs %}
