---
title: A - Lucky Seven
layout: page
parent: Solution
---

# A - Lucky Seven

## Codes

### C

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

### C++

```cpp
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

### Java

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

### Python

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
