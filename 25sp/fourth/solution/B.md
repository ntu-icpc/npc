---
title: B - Glass Bridge
layout: page
parent: NPC25 The 4th Contest Solution
---

# B - Glass Bridge

## Subtask 1
{% tabs FourthB1 %}
{% tab FourthB1 C %}
```c
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>

/**
 * @param n: number of players
 * @param m: number of levels
 * @param k: number of glass per level
 * @param favors: an array consisting n integers where the i-th integer is the
 * favourite number of i-th player
 * @param reals: an array consisting m integers where the i-th integer is the
 * number (range from 1 to k) of the only real glass in i-th level
 * @return: return an integer representing maximum number of players survive
 */
int solve(int n, int m, int k, int *favors, int *reals) {
  bool *lives = (bool *)malloc(n * sizeof(bool));
  for (int i = 0; i < n; ++i) {
    lives[i] = true;
  }
  int num_lives = n;

  for (int level = 0; level < m; ++level) {
    for (int player = 0; player < n; ++player) {
      if (!lives[player]) {
        continue;
      }
      if (favors[player] == reals[level]) {
        break;
      } else {
        lives[player] = false;
        num_lives--;
      }
    }
  }
  free(lives);
  return num_lives;
}

int main() {
  int n, m, k;
  scanf("%d %d %d", &n, &m, &k);
  int *favors = (int *)calloc(n, sizeof(int));
  int *reals = (int *)calloc(m, sizeof(int));

  for (int i = 0; i < n; ++i) {
    scanf("%d", &favors[i]);
  }
  for (int i = 0; i < m; ++i) {
    scanf("%d", &reals[i]);
  }
  int ans = solve(n, m, k, favors, reals);
  printf("%d\n", ans);
  free(favors);
  free(reals);
  return 0;
}
```
{% endtab %}
{% tab FourthB1 Python %}
```python
from typing import List

# If you want to use a deep recursion, you need to increase the recursion limit
# Python's default recursion limit is 1000
# Uncomment the following two lines if you need to use deep recursion
# import sys
# sys.setrecursionlimit(10**8)


class Solution:
    @staticmethod
    def solve(n: int, m: int, k: int, favors: List[int], reals: List[int]) -> int:
        """
        Args:
            n (int): number of players
            m (int): number of levels
            k (int): number of glass per level
            favors (list[int]): an array consisting n integers where the i-th integer is the favourite number of i-th player
            reals (list[int]): an array consisting m integers where the i-th integer is the number (range from 1 to k) of the only real glass in i-th level

        Returns:
            int: return an integer representing maximum number of players survive
        """
        lives = [True] * n
        num_lives = n
        for level in range(k):
            for player in range(n):
                if not lives[player]:
                    continue
                if favors[player] == reals[level]:
                    break
                else:
                    lives[player] = False
                    num_lives -= 1
        return num_lives


if __name__ == "__main__":
    n, m, k = tuple(map(int, input().split()))
    favors = list(map(int, input().split()))
    reals = list(map(int, input().split()))
    solution = Solution.solve(n, m, k, favors, reals)
    print(solution)
```
{% endtab %}
{% endtabs %}

## Solution

{% tabs FourthB %}
{% tab FourthB Python %}
```python
from typing import List

# If you want to use a deep recursion, you need to increase the recursion limit
# Python's default recursion limit is 1000
# Uncomment the following two lines if you need to use deep recursion
# import sys
# sys.setrecursionlimit(10**8)


class Solution:
    @staticmethod
    def solve(n: int, m: int, k: int, favors: List[int], reals: List[int]) -> int:
        """
        Args:
            n (int): number of players
            m (int): number of levels
            k (int): number of glass per level
            favors (list[int]): an array consisting n integers where the i-th integer is the favourite number of i-th player
            reals (list[int]): an array consisting m integers where the i-th integer is the number (range from 1 to k) of the only real glass in i-th level

        Returns:
            int: return an integer representing maximum number of players survive
        """
        current_layer = 0
        for player in range(n):
            while current_layer < m and favors[player] == reals[current_layer]:
                current_layer += 1
            if current_layer == m:
                return n - player
        return 0


if __name__ == "__main__":
    n, m, k = tuple(map(int, input().split()))
    favors = list(map(int, input().split()))
    reals = list(map(int, input().split()))
    solution = Solution.solve(n, m, k, favors, reals)
    print(solution)
```
{% endtab %}
{% tab FourthB C %}
```c
#include <stdio.h>
#include <stdlib.h>

/**
 * @brief Solves the glass bridge problem.
 *
 * @param n Number of players.
 * @param m Number of levels.
 * @param k Number of glass panes per level.
 * @param favors An array of n integers where the i-th integer is the favorite
 * number of the i-th player.
 * @param reals An array of m integers where the i-th integer is the number
 * (range from 1 to k) of the only real glass in the i-th level.
 * @return An integer representing the maximum number of players that survive.
 */
int solve(int n, int m, int k, int* favors, int* reals) {
  int current_layer = 0;
  for (int player = 0; player < n; player++) {
    while (current_layer < m && favors[player] == reals[current_layer]) {
      current_layer++;
    }
    if (current_layer == m) {
      return n - player;
    }
  }
  return 0;
}

int main() {
  int n, m, k;
  scanf("%d %d %d", &n, &m, &k);

  int* favors = (int*)malloc(n * sizeof(int));
  if (favors == NULL) {
    fprintf(stderr, "Memory allocation failed for favors\n");
    return 1;
  }
  for (int i = 0; i < n; i++) {
    scanf("%d", &favors[i]);
  }

  int* reals = (int*)malloc(m * sizeof(int));
  if (reals == NULL) {
    fprintf(stderr, "Memory allocation failed for reals\n");
    free(favors);
    return 1;
  }
  for (int i = 0; i < m; i++) {
    scanf("%d", &reals[i]);
  }

  int result = solve(n, m, k, favors, reals);
  printf("%d\n", result);

  free(favors);
  free(reals);

  return 0;
}

```
{% endtab %}
{% tab FourthB C++ %}
```cpp
#include <iostream>
#include <numeric>
#include <vector>

/**
 * @class Solution
 * @brief Encapsulates the logic for the glass bridge problem.
 */
class Solution {
 public:
  /**
   * @brief Solves the glass bridge problem.
   *
   * @param n The number of players.
   * @param m The number of levels.
   * @param k The number of glass panes per level.
   * @param favors A constant reference to a vector of integers representing the
   * favorite numbers of the players.
   * @param reals A constant reference to a vector of integers representing the
   * real glass pane numbers for each level.
   * @return An integer representing the maximum number of players that survive.
   */
  static int solve(int n, int m, int k, const std::vector<int>& favors,
                   const std::vector<int>& reals) {
    int current_layer = 0;
    for (int player = 0; player < n; ++player) {
      while (current_layer < m && favors[player] == reals[current_layer]) {
        current_layer++;
      }
      if (current_layer == m) {
        return n - player;
      }
    }
    return 0;
  }
};

int main() {
  std::ios::sync_with_stdio(false);
  std::cin.tie(nullptr);
  std::cout.tie(nullptr);
  int n, m, k;
  std::cin >> n >> m >> k;
  std::vector<int> favors(n), reals(m);
  for (int i = 0; i < n; ++i) {
    std::cin >> favors[i];
  }
  for (int i = 0; i < m; ++i) {
    std::cin >> reals[i];
  }
  int ans = Solution::solve(n, m, k, favors, reals);
  std::cout << ans << "\n";
  return 0;
}
```
{% endtab %}
{% tab FourthB Java %}
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.List;
import java.util.StringTokenizer;

/**
 * @class Solution
 * @brief Contains the solution for the glass bridge problem.
 */
public class Solution {
  /**
   * Solves the glass bridge problem.
   *
   * @param n      The number of players.
   * @param m      The number of levels.
   * @param k      The number of glass panes per level.
   * @param favors A list of integers representing the favorite numbers of the players.
   * @param reals  A list of integers representing the real glass pane numbers for each level.
   * @return An integer representing the maximum number of players that survive.
   */
  public static int solve(int n, int m, int k, List<Integer> favors, List<Integer> reals) {
    int currentLayer = 0;
    for (int player = 0; player < n; player++) {
      while (currentLayer < m && favors.get(player).equals(reals.get(currentLayer))) {
        currentLayer++;
      }
      if (currentLayer == m) {
        return n - player;
      }
    }
    return 0;
  }

  public static void main(String[] args) throws IOException {
    BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

    StringTokenizer st = new StringTokenizer(reader.readLine());
    int n = Integer.parseInt(st.nextToken());
    int m = Integer.parseInt(st.nextToken());
    int k = Integer.parseInt(st.nextToken());

    List<Integer> favors = new ArrayList<>(n);
    st = new StringTokenizer(reader.readLine());
    for (int i = 0; i < n; i++) {
      favors.add(Integer.parseInt(st.nextToken()));
    }

    List<Integer> reals = new ArrayList<>(m);
    st = new StringTokenizer(reader.readLine());
    for (int i = 0; i < m; i++) {
      reals.add(Integer.parseInt(st.nextToken()));
    }

    int result = solve(n, m, k, favors, reals);
    System.out.println(result);
  }
}

```
{% endtab %}
{% tab FourthB Rust %}
```rust
use std::io::{self, BufRead};

/// Solves the glass bridge problem.
///
/// # Arguments
///
/// * `n` - The number of players.
/// * `m` - The number of levels.
/// * `_k` - The number of glass panes per level (unused in the logic).
/// * `favors` - A slice of i32 representing the favorite numbers of the players.
/// * `reals` - A slice of i32 representing the real glass pane numbers for each level.
///
/// # Returns
///
/// An i32 representing the maximum number of players that survive.
fn solve(n: i32, m: i32, _k: i32, favors: &[i32], reals: &[i32]) -> i32 {
    let mut current_layer = 0;
    for player in 0..n as usize {
        while current_layer < m as usize && favors[player] == reals[current_layer] {
            current_layer += 1;
        }
        if current_layer == m as usize {
            return n - player as i32;
        }
    }
    0
}

fn main() -> io::Result<()> {
    let stdin = io::stdin();
    let mut lines = stdin.lock().lines();

    let first_line = lines.next().unwrap()?;
    let mut params = first_line.split_whitespace();
    let n: i32 = params.next().unwrap().parse().unwrap();
    let m: i32 = params.next().unwrap().parse().unwrap();
    let k: i32 = params.next().unwrap().parse().unwrap();

    let second_line = lines.next().unwrap()?;
    let favors: Vec<i32> = second_line
        .split_whitespace()
        .map(|s| s.parse().unwrap())
        .collect();

    let third_line = lines.next().unwrap()?;
    let reals: Vec<i32> = third_line
        .split_whitespace()
        .map(|s| s.parse().unwrap())
        .collect();

    let result = solve(n, m, k, &favors, &reals);
    println!("{}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}