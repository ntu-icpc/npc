---
title: A - Data Compression
layout: page
parent: NPC25 Welcome AY25/26 Contest Solution
---

# A - Data Compression

{% tabs ThirdA %}
<!-- {% tab ThirdA Python %}
```python
class Solution:
    def __init__(self, value):
        self.C = value

    def setDirection(self, x: int):
        """
        Args:
            x (int): The parameter for SET_DIRECTION Operation
        """
        # Implement your solution here
        pass

    def setOffset(self, x: int):
        """
        Args:
            x (int): The parameter for SET_OFFSET Operation
        """
        # Implement your solution here
        pass

    def setId(self, x: int):
        """
        Args:
            x (int): The parameter for SET_ID Operation
        """
        # Implement your solution here
        pass

    def setGroup(self, x: int):
        """
        Args:
            x (int): The parameter for SET_GROUP Operation
        """
        # Implement your solution here
        pass

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
```
{% endtab %} -->
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
<!-- {% tab ThirdA Java %}
```java
```
{% endtab %} -->
{% endtabs %}