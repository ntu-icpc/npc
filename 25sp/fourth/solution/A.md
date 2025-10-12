---
title: A - Card
layout: page
parent: NPC25 The 4th Contest Solution
---

# A - Card

## Subtask 1

```c
#include <stdio.h>

int main() {
  int n;
  scanf("%d", &n);
  int sum = 0;
  for (int i = 0; i < n; i++) {
    int num;
    scanf("%d", &num);
    sum += num;
  }
  printf("%d\n", sum);
  return 0;
}
```

```cpp
#include <iostream>

int main() {
  std::ios_base::sync_with_stdio(false);
  std::cin.tie(nullptr);
  std::cout.tie(nullptr);
  int n;
  std::cin >> n;
  int sum = 0;
  for (int i = 0; i < n; i++) {
    int num;
    std::cin >> num;
    sum += num;
  }
  std::cout << sum << std::endl;
  return 0;
}
```

```java
import java.util.Scanner;

public class Solution_50 {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    int n = sc.nextInt();
    long sum = 0;
    for (int i = 0; i < n; i++) {
      sum += sc.nextInt();
    }
    System.out.println(sum);
    sc.close();
  }
}
```

```python
if __name__ == "__main__":
    n = int(input())
    print(sum(map(int, input().split())))
```

```rust
use std::io;

fn main() {
    let mut input = String::new();
    io::stdin().read_line(&mut input).unwrap();
    let n: i32 = input.trim().parse().unwrap();

    input.clear();
    io::stdin().read_line(&mut input).unwrap();
    let sum: i32 = input
        .split_whitespace()
        .map(|s| s.parse::<i32>().unwrap())
        .sum();

    println!("{}", sum);
}
```

## Solution

{% tabs FourthA %}
{% tab FourthA Python %}
```python
class Solution:
    @staticmethod
    def solve(n: int, cards: list[int]) -> int:
        return sum([card for card in cards if card > 0])


if __name__ == "__main__":
    n = int(input())
    cards = list(map(int, input().split()))
    print(Solution.solve(n, cards))
```
{% endtab %}
{% tab FourthA C %}
```c
#include <stdio.h>
#include <stdlib.h>

int solve(int n, int *cards) {
  int sum = 0;
  for (int i = 0; i < n; i++) {
    if (cards[i] > 0) {
      sum += cards[i];
    }
  }
  return sum;
}

int main() {
  int n;
  scanf("%d", &n);
  int *cards = (int *)calloc(n, sizeof(int));
  for (int i = 0; i < n; i++) {
    scanf("%d", &cards[i]);
  }
  printf("%d\n", solve(n, cards));
  free(cards);
  return 0;
}
```
{% endtab %}
{% tab FourthA C++ %}
```cpp
#include <iostream>
#include <numeric>
#include <vector>

class Solution {
 public:
  static int solve(int n, const std::vector<int>& cards) {
    int sum = 0;
    for (int card : cards) {
      if (card > 0) {
        sum += card;
      }
    }
    return sum;
  }
};

int main() {
  std::ios_base::sync_with_stdio(false);
  std::cin.tie(nullptr);
  std::cout.tie(nullptr);
  int n;
  std::cin >> n;
  std::vector<int> cards(n);
  for (int i = 0; i < n; ++i) {
    std::cin >> cards[i];
  }
  std::cout << Solution::solve(n, cards) << std::endl;
  return 0;
}
```
{% endtab %}
{% tab FourthA Java %}
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Solution {
  public static int solve(int n, List<Integer> cards) {
    int sum = 0;
    for (int card : cards) {
      if (card > 0) {
        sum += card;
      }
    }
    return sum;
  }

  public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    int n = scanner.nextInt();
    List<Integer> cards = new ArrayList<>();
    for (int i = 0; i < n; i++) {
      cards.add(scanner.nextInt());
    }
    System.out.println(solve(n, cards));
    scanner.close();
  }
}
```
{% endtab %}
{% tab FourthA Rust %}
```rust
use std::io;

fn solve(_n: usize, cards: &[i32]) -> i32 {
    cards.iter().filter(|&&card| card > 0).sum()
}

fn main() {
    let mut input = String::new();
    io::stdin().read_line(&mut input).unwrap();
    let n: usize = input.trim().parse().unwrap();

    input.clear();
    io::stdin().read_line(&mut input).unwrap();
    let cards: Vec<i32> = input
        .split_whitespace()
        .map(|s| s.parse().unwrap())
        .collect();

    println!("{}", solve(n, &cards));
}
```
{% endtab %}
{% endtabs %}