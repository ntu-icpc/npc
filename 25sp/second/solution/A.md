---
title: A - Triangle
layout: page
parent: Solutions
---

# A - Triangle

## $$a=b$$

We can draw:

![](imgs/A/sum.svg)

So the answer is:

$$
s = (a + 1) + a + \cdots + 1 = \frac{(a + 2)(a + 1)}{2}
$$

{% tabs Code %}
{% tab Code Python %}
```python
class Solution:
    @staticmethod
    def solve(a: int, b: int) -> int:
        return (a + 1) * (b + 2) // 2


if __name__ == "__main__":
    a, b = map(int, input().split())
    print(Solution.solve(a, b))
```
{% endtab %}
{% tab Code C %}
#include <stdio.h>

int64_t solve(int a, int b);

int main() {
  int a, b;
  scanf("%d%d", &a, &b);
  printf("%" PRId64 "\n", solve(a, b));
  return 0;
}

int64_t solve(int a, int b) { return (int64_t)(a + 1) * (b + 2) / 2; }
```
{% endtab %}
{% tab Code C++ %}
```cpp
#include <iostream>

int64_t solve(int a, int b);

int main() {
  int a, b;
  std::cin >> a >> b;
  std::cout << solve(a, b) << std::endl;
  return 0;
}

int64_t solve(int a, int b) { return (int64_t)(a + 1) * (b + 2) / 2; }
```
{% endtab %}
{% tab Code Java %}
```java
import java.util.Scanner;

public class Solution {
  static long solve(int a, int b) {
    return (long) (a + 1) * (b + 2) / 2;
  }

  public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    int a = scanner.nextInt();
    int b = scanner.nextInt();
    System.out.println(solve(a, b));
  }
}
```
{% endtab %}
{% tab Code Go %}
```go
package main

import "fmt"

func solve(a, b int) int {
	return (a + 1) * (b + 2) / 2
}

func main() {
	var a, b int
	fmt.Scan(&a, &b)
	fmt.Println(solve(a, b))
}
```
{% endtab %}
{% endtabs %}

## GCD Solutions

![](imgs/A/sollog.svg)

{% tabs Code %}

{% tab Code Java %}
```java
import java.util.Scanner;

public class Solution {
  public static int gcd(int a, int b) {
    while (b != 0) {
      int temp = b;
      b = a % b;
      a = temp;
    }
    return a;
  }

  public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    int a = scanner.nextInt();
    int b = scanner.nextInt();
    System.out.println(((long) (a + 1) * (b + 1) + gcd(a, b) + 1) / 2);
    scanner.close();
  }
}
```
{% endtab %}

{% tab Code Go %}
```go
package main

import "fmt"

func gcd(a, b int) int {
	for b != 0 {
		a, b = b, a%b
	}
	return a
}

func solve(a, b int) int {
	return ((a+1)*(b+1) + gcd(a, b) + 1) / 2
}

func main() {
	var a, b int
	fmt.Scan(&a, &b)
	fmt.Println(solve(a, b))
}
```
{% endtab %}
{% endtabs %}
