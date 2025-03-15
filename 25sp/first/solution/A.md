---
title: A - Lucky Seven
layout: page
parent: Solutions
---

# A - Lucky Seven

## Analysis

Just `print(s[6])` because the string stored in most language are in 0-indexed.

## Codes

{% tabs Code %}

{% tab Code C %}
```c
#include <stdio.h>

/**
 * This function returns Bob's lucky letter.
 *
 * @param s A character array of length 10 representing the letters Bob saw.
 * @return The 7-th letter in the string, which is Bob's lucky letter.
 */
char solve(char *s) { return s[6]; }

int main() {
  char s[10];
  scanf("%s", s);
  printf("%c\n", solve(s));
  return 0;
}
```
{% endtab %}

{% tab Code C++ %}
```cpp
#include <iostream>
#include <string>

class Solution {
 public:
  /**
   * This function returns Bob's lucky letter.
   *
   * @param s A string of length 10 representing the letters Bob saw.
   * @return The 7-th letter in the string, which is Bob's lucky letter.
   */
  static char solve(const std::string &s) { return s[6]; }
};

int main() {
  std::string s;
  std::cin >> s;
  std::cout << Solution::solve(s) << std::endl;
  return 0;
}
```
{% endtab %}

{% tab Code Java %}
```java
import java.util.Scanner;

class Solution {
  /**
   * @param s: the string of length 10 representing the letters Bob saw
   * @return: the 7-th letter in the string, which is Bob's lucky letter
   */
  public static char solve(String s) {
    return s.charAt(6);
  }

  public static void main(String[] args) throws java.lang.Exception {
    Scanner input = new Scanner(System.in);
    String s = input.nextLine();
    System.out.println(solve(s));
  }
}
```
{% endtab %}

{% tab Code Python %}
```python
class Solution:
    @staticmethod
    def solve(s: str) -> str:
        """
        Returns Bob's lucky letter.

        Args:
            s (str): The string of length 10 representing the letters Bob saw.

        Returns:
            str: The 7-th letter in the string, which is Bob's lucky letter.
        """
        return s[6]


if __name__ == "__main__":
    s = input()
    print(Solution.solve(s))
```
{% endtab %}

{% endtabs %}
