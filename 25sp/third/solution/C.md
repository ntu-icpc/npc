---
title: C - Contact
layout: page
parent: NPC25 Welcome AY25/26 Contest Solution
---

# C - Contact

## Rolling hash Solution

{% tabs ThirdC %}
{% tab ThirdC Python %}
```python
from typing import List

# If you want to use a deep recursion, you need to increase the recursion limit
# Python's default recursion limit is 1000
# Uncomment the following two lines if you need to use deep recursion
# import sys
# sys.setrecursionlimit(10**8)


class Solution:
    @staticmethod
    def solve(n: int, phones: List[str], q: int, queries: List[str]) -> List[int]:
        """
        Args:
            n (int): number of phone numbers
            phones (list[str]): array of strings representing phone number
            q (int): number of queries
            queries (list[str]): array of strings representing queries

        Returns:
            list[int]: an array of size q where the i-th index is the answer to i-th query
        """
        hashing = {}
        for s in phones:
            hash = 0
            for c in s:
                hash *= 13
                hash += int(c) + 1
                hash %= 9223372036854775783

                if hashing.get(hash) is None:
                    hashing[hash] = 0
                hashing[hash] += 1

        ans = []
        for s in queries:
            hash = 0
            for c in s:
                hash *= 13
                hash += int(c) + 1
                hash %= 9223372036854775783

            if hashing.get(hash) is None:
                ans.append(0)
            else:
                ans.append(hashing[hash])
        return ans


if __name__ == "__main__":
    n, q = tuple(map(int, input().split()))
    phones = [input() for i in range(n)]
    queries = [input() for i in range(q)]
    solution = Solution.solve(n, phones, q, queries)
    for ans in solution:
        print(ans)
```
{% endtab %}
{% tab ThirdC C++ %}
```cpp
#include <iostream>
#include <string>
#include <unordered_map>
#include <vector>
using namespace std;

class Solution {
 public:
  /**
   * @param n: number of phone numbers
   * @param phones: array of strings representing phone number
   * @param q: number of queries
   * @param queries: vector of strings representing queries
   * @return: return an array of size q where the i-th index is the answer to
   * i-th query
   */
  static std::vector<int> solve(int n, std::vector<std::string>& phones, int q,
                                std::vector<std::string>& queries) {
    // Implement your solution by completing the following function
    unordered_map<unsigned long long, int> st;
    for (int i = 0; i < n; i++) {
      string s = phones[i];
      unsigned long long hash = 0;
      for (char c : s) {
        hash *= 11;
        hash += c - '0' + 1;

        if (st.find(hash) == st.end()) st[hash] = 0;
        st[hash] += 1;
      }
    }
    vector<int> ans(q, 0);
    for (int i = 0; i < q; i++) {
      string s = queries[i];
      unsigned long long hash = 0;
      for (char c : s) {
        hash *= 11;
        hash += c - '0' + 1;
      }
      // cout << hash << " ";
      if (st.find(hash) != st.end()) ans[i] = st[hash];
    }
    return ans;
  }
};

int main() {
  int n, q;
  std::cin >> n >> q;
  std::vector<std::string> phones(n);
  for (int i = 0; i < n; ++i) std::cin >> phones[i];
  std::vector<std::string> queries(q);
  for (int i = 0; i < n; ++i) std::cin >> queries[i];
  std::vector<int> ans = Solution::solve(n, phones, q, queries);
  for (auto x : ans) {
    std::cout << x << '\n';
  }
  std::cout << "\n";
  return 0;
}
```
{% endtab %}
{% tab ThirdC Java %}
```java
import java.util.HashMap;
import java.util.Scanner;

class Solution {
  /**
   * @param n: number of phone numbers
   * @param phones: array of strings representing phone number
   * @param q: number of queries
   * @param queries: vector of strings representing queries
   * @return: return an array of size q where the i-th index is the answer to i-th query
   */
  static int[] solve(int n, String[] phones, int q, String[] queries) {
    // Implement your solution here
    HashMap<Long, Integer> hashing = new HashMap<>();
    for (String s : phones) {
      long hash = 0;
      for (char c : s.toCharArray()) {
        hash *= 13;
        hash += c - '0' + 1;

        if (!hashing.containsKey(hash))
          hashing.put(hash, 0);
        hashing.put(hash, hashing.get(hash) + 1);
      }
    }

    int[] ans = new int[q];
    for (int i = 0; i < q; i++) {
      String s = queries[i];
      long hash = 0;
      for (char c : s.toCharArray()) {
        hash *= 13;
        hash += c - '0' + 1;
      }
      if (!hashing.containsKey(hash))
        ans[i] = 0;
      else
        ans[i] = hashing.get(hash);
    }
    return ans;
  }

  public static void main(String[] args) throws java.lang.Exception {
    Scanner input = new Scanner(System.in);

    int n = input.nextInt();
    int q = input.nextInt();
    input.nextLine();
    String[] phones = new String[n];
    for (int i = 0; i < n; i++) {
      phones[i] = input.nextLine();
    }

    String[] queries = new String[q];
    for (int i = 0; i < q; i++) {
      queries[i] = input.nextLine();
    }

    int[] ans = solve(n, phones, q, queries);
    for (int i = 0; i < q; i++) {
      System.out.println(ans[i]);
    }
    System.out.println();

    input.close();
  }
}
```
{% endtab %}
{% endtabs %}


## Binary Search
```cpp
#include <iostream>
#include <string>
#include <unordered_map>
#include <algorithm>
#include <vector>
using namespace std;

class Solution {
 public:
  /**
   * @param n: number of phone numbers
   * @param phones: array of strings representing phone number
   * @param q: number of queries
   * @param queries: vector of strings representing queries
   * @return: return an array of size q where the i-th index is the answer to
   * i-th query
   */
  static std::vector<int> solve(int n, std::vector<std::string>& phones, int q,
                                std::vector<std::string>& queries) {
    // Implement your solution by completing the following function
    sort(phones.begin(), phones.end());
    vector<int> ans(q);
    for (int i = 0; i < q; i++) {
      string s = queries[i];
      int low = 0, high = n;
      int idx = 0;
      for (char c : s) {
        int l = low, r = high;
        while (l < r) {
          int m = (l + r) / 2;

          if (phones[m].size() <= idx || phones[m][idx] < c)
            l = m + 1;
          else
            r = m;
        }

        low = l;

        r = high;
        while (l < r) {
          int m = (l + r) / 2;
          if (phones[m].size() <= idx || phones[m][idx] <= c)
            l = m + 1;
          else
            r = m;
        }
        high = l;
        idx++;
      }
      ans[i] = high - low;
    }
    return ans;
  }
};

int main() {
  int n, q;
  std::cin >> n >> q;
  std::vector<std::string> phones(n);
  for (int i = 0; i < n; ++i) std::cin >> phones[i];
  std::vector<std::string> queries(q);
  for (int i = 0; i < n; ++i) std::cin >> queries[i];
  std::vector<int> ans = Solution::solve(n, phones, q, queries);
  for (auto x : ans) {
    std::cout << x << '\n';
  }
  std::cout << "\n";
  return 0;
}
```



## Trie

```cpp
#include <bits/stdc++.h>
using namespace std;

struct node {
  node* nxt[10];
  int cnt = 0;
  node() {
    for (int i = 0; i < 10; i++) nxt[i] = NULL;
  }
};

int main() {
  ios_base::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);
  node* root = new node();
  int n, q;
  cin >> n >> q;

  for (int i = 0; i < n; i++) {
    string s;
    cin >> s;
    node* cur = root;
    for (char c : s) {
      if (cur->nxt[c - '0'] == NULL) cur->nxt[c - '0'] = new node();
      cur = cur->nxt[c - '0'];
      cur->cnt += 1;
    }
  }

  for (int i = 0; i < q; i++) {
    string s;
    cin >> s;
    node* cur = root;
    int ans = 0;
    for (char c : s) {
      cur = cur->nxt[c - '0'];
      if (cur == NULL) break;
    }
    if (cur != NULL) ans = cur->cnt;
    cout << ans << "\n";
  }
}
```
