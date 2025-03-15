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

{% raw %}
<div class="code-tabs">
  <div class="tab-buttons">
    <button class="tab-button active" data-tab="python">Python</button>
    <button class="tab-button" data-tab="typescript">TypeScript</button>
    <button class="tab-button" data-tab="curl">cURL</button>
  </div>
  
  <div id="tab-python" class="tab-content active">
{% endraw %}

```python
print("Hello, World!")
```

{% raw %}
  </div>
  <div id="tab-typescript" class="tab-content">
{% endraw %}

```typescript
console.log("Hello, World!");
```

{% raw %}
  </div>
  <div id="tab-curl" class="tab-content">
{% endraw %}

```sh
curl https://example.com
```

{% raw %}
  </div>
</div>

<script>
  document.addEventListener('DOMContentLoaded', function() {
    const tabButtons = document.querySelectorAll('.tab-button');
    
    tabButtons.forEach(button => {
      button.addEventListener('click', function() {
        const tab = this.getAttribute('data-tab');
        
        // Deactivate all tabs
        document.querySelectorAll('.tab-content').forEach(content => {
          content.classList.remove('active');
        });
        
        document.querySelectorAll('.tab-button').forEach(btn => {
          btn.classList.remove('active');
        });
        
        // Activate the selected tab
        document.getElementById('tab-' + tab).classList.add('active');
        this.classList.add('active');
      });
    });
  });
</script>
{% endraw %}

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
