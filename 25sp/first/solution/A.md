---
title: A - Lucky Seven
layout: page
parent: Solution
---

# A - Lucky Seven

## Solution

**Abridged Statement**: Print the 7-th character.

Just `print(s[6])` because the string stored in most language are in 0-indexed.

## Codes

<style>
  .code-tabs {
    background: #1e1e1e;
    padding: 10px;
    border-radius: 8px;
  }
  .code-tabs .tab-buttons {
    display: flex;
    gap: 10px;
    margin-bottom: 10px;
  }
  .code-tabs .tab-buttons button {
    background: none;
    border: none;
    padding: 8px 12px;
    cursor: pointer;
    font-weight: bold;
    color: #888;
  }
  .code-tabs .tab-buttons button.active {
    color: orange;
    border-bottom: 2px solid orange;
  }
  .code-tabs .tab-content {
    display: none;
  }
  .code-tabs .tab-content.active {
    display: block;
  }
</style>

<div class="code-tabs">
  <div class="tab-buttons">
    <button class="active" onclick="switchTab('tab-python', this)">Python</button>
    <button onclick="switchTab('tab-typescript', this)">TypeScript</button>
    <button onclick="switchTab('tab-curl', this)">cURL</button>
  </div>
  <div id="tab-python" class="tab-content active">
    ```python
    print("Hello, World!")
    ```
  </div>
  <div id="tab-typescript" class="tab-content">
    ```typescript
    console.log("Hello, World!");
    ```
  </div>
  <div id="tab-curl" class="tab-content">
    ```sh
    curl https://example.com
    ```
  </div>
</div>

<script>
  function switchTab(tabId, button) {
    document.querySelectorAll(".code-tabs .tab-content").forEach(tab => {
      tab.classList.remove("active");
    });
    document.querySelector("#" + tabId).classList.add("active");

    document.querySelectorAll(".code-tabs .tab-buttons button").forEach(btn => {
      btn.classList.remove("active");
    });
    button.classList.add("active");
  }
</script>

<details markdown="block">
<summary>C</summary>
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
</details>

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
