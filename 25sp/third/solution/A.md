---
title: A - Data Compression
layout: page
parent: NPC25 Welcome AY25/26 Contest Solution
---

# A - Data Compression

{% tabs ThirdA %}
{% tab ThirdA Python %}
```python
# If you want to use a deep recursion, you need to increase the recursion limit
# Python's default recursion limit is 1000
# Uncomment the following two lines if you need to use deep recursion
# import sys
# sys.setrecursionlimit(10**8)


class Solution:
    def __init__(self, value):
        self.C = value

    def setDirection(self, x: int):
        """
        Args:
            x (int): The parameter for SET_DIRECTION Operation
        """
        # Implement your solution here
        mask = (1 << 2) - 1
        self.C &= ~mask
        self.C |= x

    def setOffset(self, x: int):
        """
        Args:
            x (int): The parameter for SET_OFFSET Operation
        """
        # Implement your solution here
        mask = ((1 << 8) - 1) << 2
        self.C &= ~mask
        self.C |= x << 2

    def setId(self, x: int):
        """
        Args:
            x (int): The parameter for SET_ID Operation
        """
        # Implement your solution here
        mask = ((1 << 10) - 1) << 10
        self.C &= ~mask
        self.C |= x << 10

    def setGroup(self, x: int):
        """
        Args:
            x (int): The parameter for SET_GROUP Operation
        """
        # Implement your solution here
        mask = ((1 << 10) - 1) << 20
        self.C &= ~mask
        self.C |= x << 20

    def getC(self) -> int:
        """
        Returns:
            int: The current value of C
        """
        return self.C


if __name__ == "__main__":
    value, q = tuple(map(int, input().split()))
    sol = Solution(value)
    for i in range(q):
        operators = input()
        if operators == "GET_C":
            print(sol.getC())
        else:
            operator, x = tuple(operators.split())
            x = int(x)
            if operator == "SET_DIRECTION":
                sol.setDirection(x)
            elif operator == "SET_OFFSET":
                sol.setOffset(x)
            elif operator == "SET_ID":
                sol.setId(x)
            elif operator == "SET_GROUP":
                sol.setGroup(x)
```
{% endtab %}
{% tab ThirdA C %}
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int C;  // initial value C

/**
 * @param x: The parameter for SET_DIRECTION Operation
 */
void setDirection(int x) {
  // Implement your solution here
  int mask = (1 << 2) - 1;
  C &= (~mask);
  C |= x;
}

/**
 * @param x: The parameter for SET_OFFSET Operation
 */
void setOffset(int x) {
  // Implement your solution here
  int mask = ((1 << 8) - 1) << 2;
  C &= (~mask);
  C |= x << 2;
}

/**
 * @param x: The parameter for SET_ID Operation
 */
void setId(int x) {
  // Implement your solution here
  int mask = ((1 << 10) - 1) << 10;
  C &= (~mask);
  C |= x << 10;
}

/**
 * @param x: The parameter for SET_GROUP Operation
 */
void setGroup(int x) {
  // Implement your solution here
  int mask = ((1<<10) - 1) << 20;
  C &= (~mask);
  C |= x << 20;
}

/**
 * @return: return the current value of C
 */
int getC() { return C; }

int main() {
  int value, q;
  scanf("%d %d", &value, &q);
  C = value;

  char* ope = (char*)malloc(15 * sizeof(char));
  char* GET_C = "GET_C";
  char* SET_DIRECTION = "SET_DIRECTION";
  char* SET_OFFSET = "SET_OFFSET";
  char* SET_ID = "SET_ID";
  char* SET_GROUP = "SET_GROUP";

  int x;
  for (int i = 0; i < q; ++i) {
    scanf("%s", ope);
    if (strcmp(ope, GET_C) == 0)
      printf("%d\n", getC());
    else {
      scanf("%d", &x);
      if (strcmp(ope, SET_DIRECTION) == 0)
        setDirection(x);
      else if (strcmp(ope, SET_OFFSET) == 0)
        setOffset(x);
      else if (strcmp(ope, SET_ID) == 0)
        setId(x);
      else if (strcmp(ope, SET_GROUP) == 0)
        setGroup(x);
    }
  }

  free(ope);
  // free(GET_C);
  // free(SET_DIRECTION);
  // free(SET_OFFSET);
  // free(SET_ID);
  // free(SET_GROUP);
  return 0;
}
```
{% endtab %}
{% tab ThirdA C++ %}
```cpp
#include <iostream>
#include <string>

class DataCompression {
 public:
  int C;  // initial value C

  DataCompression(int value) { this->C = value; }

  /**
   * @param x: The parameter for SET_DIRECTION Operation
   */
  void setDirection(int x) {
    // Implement your solution here
    int mask = (1 << 2) - 1;
    C &= (~mask);
    C |= x;
  }

  /**
   * @param x: The parameter for SET_OFFSET Operation
   */
  void setOffset(int x) {
    // Implement your solution here
    int mask = ((1 << 8) - 1) << 2;
    C &= (~mask);
    C |= x << 2;
  }

  /**
   * @param x: The parameter for SET_ID Operation
   */
  void setId(int x) {
    // Implement your solution here
    int mask = ((1 << 10) - 1) << 10;
    C &= (~mask);
    C |= x << 10;
  }

  /**
   * @param x: The parameter for SET_GROUP Operation
   */
  void setGroup(int x) {
    // Implement your solution here
    int mask = ((1<<10) - 1) << 20;
    C &= (~mask);
    C |= x << 20;
}

  /**
   * @return: return the current value of C
   */
  int getC() { return this->C; }
};

int main() {
  int value, q;
  std::cin >> value >> q;
  DataCompression* data = new DataCompression(value);
  for (int i = 0; i < q; ++i) {
    std::string ope;
    std::cin >> ope;
    if (ope == "GET_C")
      std::cout << data->getC() << "\n";
    else {
      int x;
      std::cin >> x;
      if (ope == "SET_DIRECTION")
        data->setDirection(x);
      else if (ope == "SET_OFFSET")
        data->setOffset(x);
      else if (ope == "SET_ID")
        data->setId(x);
      else if (ope == "SET_GROUP")
        data->setGroup(x);
    }
  }
  return 0;
}

```
{% endtab %}
{% tab ThirdA Java %}
```java
import java.util.Scanner;

class DataCompression {
  int C; // initial value C

  public DataCompression(int value) {
    this.C = value;
  }

  /**
   * @param x: The parameter for SET_DIRECTION Operation
   */
  void setDirection(int x) {
    // Implement your solution here
    int mask = (1 << 2) - 1;
    this.C &= ~mask;
    this.C |= x;
  }

  /**
   * @param x: The parameter for SET_OFFSET Operation
   */
  void setOffset(int x) {
    // Implement your solution here
    int mask = ((1 << 8) - 1) << 2;
    this.C &= ~mask;
    this.C |= x << 2;
  }

  /**
   * @param x: The parameter for SET_ID Operation
   */
  void setId(int x) {
    // Implement your solution here
    int mask = ((1 << 10) - 1) << 10;
    this.C &= ~mask;
    this.C |= x << 10;
  }

  /**
   * @param x: The parameter for SET_GROUP Operation
   */
  void setGroup(int x) {
    // Implement your solution here
    int mask = ((1 << 10) - 1) << 20;
    this.C &= ~mask;
    this.C |= x << 20;
  }

  /**
   * @return: return the current value of C
   */
  int getC() {
    return this.C;
  }
}

class Solution {
  public static void main(String[] args) throws java.lang.Exception {
    Scanner input = new Scanner(System.in);

    int C = input.nextInt();
    int q = input.nextInt();

    DataCompression data = new DataCompression(C);

    for (int i = 0; i < q; i++) {
      String operator = input.next();
      if (operator.equals("GET_C"))
        System.out.println(data.getC());
      else {
        int x = input.nextInt();
        if (operator.equals("SET_DIRECTION"))
          data.setDirection(x);
        else if (operator.equals("SET_OFFSET"))
          data.setOffset(x);
        else if (operator.equals("SET_ID"))
          data.setId(x);
        else if (operator.equals("SET_GROUP"))
          data.setGroup(x);
      }
      
    }
    input.close();
  }
}
```
{% endtab %}
{% endtabs %}