---
title: C - Contact
layout: page
parent: Solutions
---

# C - Contact

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
{% tab ThirdC C %}
```c
#include <ctype.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// --- Hash Map Implementation ---
#define HASH_MAP_SIZE 100003  // A reasonably large prime number

typedef struct Node {
  unsigned long long key;
  int value;
  struct Node* next;
} Node;

Node* buckets[HASH_MAP_SIZE];

void init_hash_map() {
  for (int i = 0; i < HASH_MAP_SIZE; i++) {
    buckets[i] = NULL;
  }
}

void free_hash_map() {
  for (int i = 0; i < HASH_MAP_SIZE; i++) {
    Node* current = buckets[i];
    while (current != NULL) {
      Node* temp = current;
      current = current->next;
      free(temp);
    }
    buckets[i] = NULL;
  }
}

void put(unsigned long long key) {
  int index = key % HASH_MAP_SIZE;
  Node* current = buckets[index];
  while (current != NULL) {
    if (current->key == key) {
      current->value++;
      return;
    }
    current = current->next;
  }
  Node* newNode = (Node*)malloc(sizeof(Node));
  newNode->key = key;
  newNode->value = 1;
  newNode->next = buckets[index];
  buckets[index] = newNode;
}

int get(unsigned long long key) {
  int index = key % HASH_MAP_SIZE;
  Node* current = buckets[index];
  while (current != NULL) {
    if (current->key == key) {
      return current->value;
    }
    current = current->next;
  }
  return 0;
}
// --- End of Hash Map Implementation ---

/**
 * @param n: number of phone numbers
 * @param phones: array of strings representing phone number
 * @param q: number of queries
 * @param queries: array of strings representing queries
 * @param result: You need to fill the result array of size q where the i-th
 * index is the answer to i-th query
 */
void solve(int n, char** phones, int q, char** queries, int* result) {
  init_hash_map();

  for (int i = 0; i < n; i++) {
    char* s = phones[i];
    unsigned long long hash = 0;
    for (int j = 0; s[j] != '\0'; j++) {
      hash *= 11;
      hash += s[j] - '0' + 1;
      put(hash);
    }
  }

  for (int i = 0; i < q; i++) {
    char* s = queries[i];
    unsigned long long hash = 0;
    for (int j = 0; s[j] != '\0'; j++) {
      hash *= 11;
      hash += s[j] - '0' + 1;
    }
    result[i] = get(hash);
  }

  free_hash_map();
}

// Helper function to read a single word (string) of arbitrary length from stdin
char* read_word() {
  size_t capacity = 16;
  char* buffer = malloc(capacity);
  if (!buffer) return NULL;

  size_t i = 0;
  int c;

  // Skip leading whitespace
  while ((c = getchar()) != EOF && isspace(c));

  if (c == EOF) {
    free(buffer);
    return NULL;
  }

  // Read the word
  do {
    if (i + 1 >= capacity) {
      capacity *= 2;
      char* new_buffer = realloc(buffer, capacity);
      if (!new_buffer) {
        free(buffer);
        return NULL;
      }
      buffer = new_buffer;
    }
    buffer[i++] = c;
    c = getchar();
  } while (c != EOF && !isspace(c));

  buffer[i] = '\0';
  return buffer;
}

int main() {
  int n, q;
  scanf("%d %d", &n, &q);

  char** phones = (char**)malloc(n * sizeof(char*));
  for (int i = 0; i < n; ++i) {
    phones[i] = read_word();
  }

  char** queries = (char**)malloc(q * sizeof(char*));
  for (int i = 0; i < q; ++i) {
    queries[i] = read_word();
  }

  int* result = (int*)calloc(q, sizeof(int));

  solve(n, phones, q, queries, result);

  for (int i = 0; i < q; i++) {
    printf("%d\n", result[i]);
  }
  printf("\n");

  // Free dynamically allocated memory
  for (int i = 0; i < n; ++i) {
    free(phones[i]);
  }
  free(phones);

  for (int i = 0; i < q; ++i) {
    free(queries[i]);
  }
  free(queries);

  free(result);

  return 0;
}
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