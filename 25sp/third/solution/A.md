---
title: A - Data Compression
layout: page
parent: NPC25 Welcome AY25/26 Contest Solution
---

# A - Data Compression

{% tabs ThirdA %}
{% tab ThirdA Python %}
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
{% endtab %}
{% tab ThirdA C++ %}
```cpp
```
{% endtab %}
{% tab ThirdA Java %}
```java
```
{% endtab %}
{% endtabs %}