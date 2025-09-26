---
title: B - Stack
layout: page
parent: NPC25 Welcome AY25/26 Contest Solution
---

# B - Stack

{% tabs ThirdB %}
{% tab ThirdB Python %}
```python
# If you want to use a deep recursion, you need to increase the recursion limit
# Python's default recursion limit is 1000
# Uncomment the following two lines if you need to use deep recursion
# import sys
# sys.setrecursionlimit(10**8)


class Solution:
    @staticmethod
    def solve(n: int, s: str) -> str:
        """
        Args:
            n (int): length of the string
            s (str): the string representing input string

        Returns:
            str: a string representing the final string
        """
        # Implement your solution here
        st = []
        
        for c in s:
            st.append(c)
            while len(st) >= 3 and st[-1] == st[-2] and st[-2] == st[-3]:
                st.pop()
                st.pop()
                st.pop()
        
        ans = ""
        for c in st:
            ans = ans + c
        return ans


if __name__ == "__main__":
    n = int(input())
    s = input()

    solution = Solution.solve(n, s)
    print(solution)

```
{% endtab %}
{% tab ThirdB C %}
```c
#include <stdio.h>
#include <stdlib.h>

/**
 * @param n: length of the string
 * @param s: the string representing input string
 * @param ans: a pointer to store a string representing the final string
 */
void solve(int n, char *s, char *ans) {
  // TODO: Implement the solution
  
  int top = 0;
  for(int i = 0; i < n; i++){
    ans[top++] = s[i];
    while(top >= 3 && ans[top - 1] == ans[top - 2] && ans[top - 2] == ans[top - 3]){
      top -= 3;
    }
  }
  ans[top] = 0;
}

int main() {
  int n;
  scanf("%d", &n);

  char *s = (char *)calloc(n, sizeof(char));
  scanf("%s", s);

  char *ans = (char *)malloc((n + 1) * sizeof(char));

  solve(n, s, ans);

  printf("%s\n", ans);
  free(s);
  free(ans);
  return 0;
}
```
{% endtab %}
{% tab ThirdB C++ %}
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

class Solution {
 public:
  /**
   * @param n: length of the string
   * @param s: the string representing input string
   * @return: a string representing the final string
   */
  static std::string solve(int n, std::string& s) {
    // Implement your solution by completing the following function
    vector<char> stack(n);
    int top = 0;
    for(int i = 0; i < n; i++){
      stack[top++] = s[i];
      while(top >= 3 && stack[top - 1] == stack[top - 2] && stack[top - 2] == stack[top - 3]){
        top -= 3;
      }
    }
    
    std::string ans = "";
    for(int i = 0; i < top; i++) ans += stack[i];
    return ans;
  }
};

int main() {
  int n;
  std::string s;
  std::cin >> n >> s;

  std::string ans = Solution::solve(n, s);
  std::cout << ans << "\n";
  return 0;
}
```
{% endtab %}
{% tab ThirdB Java %}
```java
import java.util.Scanner;

class Solution {
  /**
   * @param n: length of the string
   * @param s: the string representing input string
   * @return: a string representing the final string
   */
  static String solve(int n, String s) {
    // Implement your solution here
    String ans = "";
    char[] stack = new char[n];
    int top = 0;
    for(int i = 0; i < n; i++){
      stack[top++] = s.charAt(i);
      while(top >= 3 && stack[top - 1] == stack[top - 2] && stack[top - 2] == stack[top - 3]){
        top -= 3;
      }
    }
    
    for(int i = 0; i < top; i++) ans += stack[i];
    return ans;
  }

  public static void main(String[] args) throws java.lang.Exception {
    Scanner input = new Scanner(System.in);

    int n = input.nextInt();
    String s = input.nextLine();
    s = input.nextLine();

    System.out.println(solve(n, s));

    input.close();
  }
}
```
{% endtab %}
{% endtabs %}