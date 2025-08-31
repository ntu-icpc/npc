---
title: E - Eating
layout: page
parent: NPC25 The 2nd Contest Solution
---

# E - Eating

## Abridged Problem Statement

You can merge two cakes $$M$$ times. And you can eat the cake by unit $$x$$ infinite times, where $$x$$ should be less than half the cake size.

But you cannot eat the cake whose size is smaller than $$K$$.

Your goal is eat the cake as much as possible.

## Observations

{: .note-title }
> Observation 1
> 
> We could merge first and eat the cakes after that.
>
> As you can eat the cake infinite times, you can eat 1 unit until the cake's size becomes $$K$$.

{: .note-title }
> Observation 2
> 
> When the cake's size become $$K$$, only one bite on this cake in the future. Thus we always eat $$K/2$$ unit in this case.

{: .note-title }
> Observation 3
>
> Maximize the cake you eat means minimize your waste.

## Greedy

{: .note-title }
> Intuitive thinking
>
> Let's denote $$w[i]$$ as the remaining unit in the end. 
>
> - $$w[i]=(k+1)/2$$ if $$a[i]>=k$$, 
> - $$w[i]=a[i]$$ if $$a[i]<k$$.
>
> To minimize the sum of $$w[i]$$, a intuitive solution here is sort the $$w[i]$$ and remove the first $$M$$ $$w[i]$$ in decreasing order.

{: .note-title }
> Issue?
>
> But there is another better choice:
>
> We can merge two cakes both of size $$k>=a[i]>(k+1)/2$$. In this case, we not only eliminated the wastage of one cake, but also decrease another cake's waste value from $$a[i]$$ to $$(k+1)/2$$.

Hence, our solution is:

- First merge all cake with size $$(k+1)/2 \leq a[i] < k$$ in decreasing order.
- After that, we eliminate the cake by merging to a large cake one by one.

{% tabs 2EGreedy %}
{% tab 2EGreedy Python %}
```python
import heapq


class Solution:
    @staticmethod
    def solve(n: int, m: int, k: int, a: list[int]) -> int:
        q1, q2 = [], []
        half = (k + 1) // 2

        for x in a:
            if x < k and x >= half:
                heapq.heappush(q1, -x)
            else:
                heapq.heappush(q2, -x)

        while m > 0 and len(q1) >= 2:
            x = -heapq.heappop(q1)
            y = -heapq.heappop(q1)
            heapq.heappush(q2, -(x + y))
            m -= 1

        while m > 0 and q1:
            x = -heapq.heappop(q2)
            y = -heapq.heappop(q1)
            heapq.heappush(q2, -(x + y))
            m -= 1

        while m > 0 and len(q2) >= 2:
            x = -heapq.heappop(q2)
            y = -heapq.heappop(q2)
            heapq.heappush(q2, -(x + y))
            m -= 1

        ans = 0
        while q2:
            x = -heapq.heappop(q2)
            if x >= k:
                ans += x - half

        return ans


if __name__ == "__main__":
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    print(Solution.solve(n, m, k, a))
```
{% endtab %}
{% tab 2EGreedy C %}
```c
#include <inttypes.h>
#include <stdint.h>
#include <stdio.h>
#include <stdlib.h>

int64_t solve(int n, int m, int k, int64_t* a);

int main() {
  int n, m, k;
  scanf("%d%d%d", &n, &m, &k);

  int64_t* a = (int64_t*)malloc(sizeof(int64_t) * n);
  for (int i = 0; i < n; i++) {
    scanf("%lld", &a[i]);
  }

  printf("%" PRId64 "\n", solve(n, m, k, a));

  free(a);
  return 0;
}

typedef struct {
  int64_t* heap;
  int size;
  int capacity;
} PriorityQueue;

PriorityQueue* createPriorityQueue(int capacity) {
  PriorityQueue* pq = (PriorityQueue*)malloc(sizeof(PriorityQueue));
  pq->heap = (int64_t*)malloc(sizeof(int64_t) * capacity);
  pq->size = 0;
  pq->capacity = capacity;
  return pq;
}

void swap(int64_t* a, int64_t* b) {
  int64_t temp = *a;
  *a = *b;
  *b = temp;
}

void heapify(PriorityQueue* pq, int idx) {
  int largest = idx;
  int left = 2 * idx + 1;
  int right = 2 * idx + 2;

  if (left < pq->size && pq->heap[left] > pq->heap[largest]) largest = left;

  if (right < pq->size && pq->heap[right] > pq->heap[largest]) largest = right;

  if (largest != idx) {
    swap(&pq->heap[idx], &pq->heap[largest]);
    heapify(pq, largest);
  }
}

void push(PriorityQueue* pq, int64_t value) {
  if (pq->size == pq->capacity) {
    pq->capacity *= 2;
    pq->heap = (int64_t*)realloc(pq->heap, sizeof(int64_t) * pq->capacity);
  }

  int i = pq->size;
  pq->heap[i] = value;
  pq->size++;

  while (i > 0 && pq->heap[(i - 1) / 2] < pq->heap[i]) {
    swap(&pq->heap[i], &pq->heap[(i - 1) / 2]);
    i = (i - 1) / 2;
  }
}

int64_t top(PriorityQueue* pq) {
  if (pq->size <= 0) {
    return -1;  // Error: empty queue
  }
  return pq->heap[0];
}

int64_t pop(PriorityQueue* pq) {
  if (pq->size <= 0) {
    return -1;  // Error: empty queue
  }

  int64_t root = pq->heap[0];
  pq->heap[0] = pq->heap[pq->size - 1];
  pq->size--;

  heapify(pq, 0);
  return root;
}

int isEmpty(PriorityQueue* pq) { return pq->size == 0; }

int size(PriorityQueue* pq) { return pq->size; }

void freePriorityQueue(PriorityQueue* pq) {
  free(pq->heap);
  free(pq);
}

int64_t solve(int n, int m, int k, int64_t* a) {
  PriorityQueue* q1 = createPriorityQueue(n);
  PriorityQueue* q2 = createPriorityQueue(n);

  int half = (k + 1) / 2;

  for (int i = 0; i < n; i++) {
    if (a[i] < k && a[i] >= half) {
      push(q1, a[i]);
    } else {
      push(q2, a[i]);
    }
  }

  while (m > 0 && size(q1) >= 2) {
    int64_t x = pop(q1);
    int64_t y = pop(q1);
    push(q2, x + y);
    m--;
  }

  while (m > 0 && !isEmpty(q1)) {
    int64_t x = pop(q2);
    int64_t y = pop(q1);
    push(q2, x + y);
    m--;
  }

  while (m > 0 && size(q2) >= 2) {
    int64_t x = pop(q2);
    int64_t y = pop(q2);
    push(q2, x + y);
    m--;
  }

  int64_t ans = 0;
  while (!isEmpty(q2)) {
    int64_t x = pop(q2);
    if (x >= k) {
      ans += x - half;
    }
  }

  freePriorityQueue(q1);
  freePriorityQueue(q2);

  return ans;
}
```
{% endtab %}
{% tab 2EGreedy C++ %}
```cpp
#include <cstdint>
#include <iostream>
#include <queue>

class Solve {
 public:
  static int64_t solve(int n, int m, int k, std::vector<int64_t> &a) {
    std::priority_queue<int64_t> q1, q2;
    int half = (k + 1) / 2;
    for (auto &x : a) {
      if (x < k && x >= half) {
        q1.push(x);
      } else {
        q2.push(x);
      }
    }
    while (m && q1.size() >= 2) {
      int64_t x = q1.top();
      q1.pop();
      int64_t y = q1.top();
      q1.pop();
      q2.push(x + y);
      m--;
    }
    while (m && !q1.empty()) {
      int64_t x = q2.top();
      q2.pop();
      int64_t y = q1.top();
      q1.pop();
      q2.push(x + y);
      m--;
    }
    while (m && q2.size() >= 2) {
      int64_t x = q2.top();
      q2.pop();
      int64_t y = q2.top();
      q2.pop();
      q2.push(x + y);
      m--;
    }
    int64_t ans = 0;
    while (!q2.empty()) {
      int64_t x = q2.top();
      q2.pop();
      if (x >= k) {
        ans += x - half;
      }
    }
    return ans;
  }
};

int main() {
  int n, m, k;
  std::cin >> n >> m >> k;
  std::vector<int64_t> a(n);
  for (auto &x : a) {
    std::cin >> x;
  }
  std::cout << Solve::solve(n, m, k, a) << std::endl;
  return 0;
}
```
{% endtab %}
{% tab 2EGreedy Java %}
```java
import java.util.Collections;
import java.util.PriorityQueue;
import java.util.Scanner;

public class Solution {
  public static long solve(int n, int m, int k, long[] a) {
    PriorityQueue<Long> q1 = new PriorityQueue<>(Collections.reverseOrder());
    PriorityQueue<Long> q2 = new PriorityQueue<>(Collections.reverseOrder());
    int half = (k + 1) / 2;
    for (int i = 0; i < n; i++) {
      if (a[i] < k && a[i] >= half) {
        q1.add(a[i]);
      } else {
        q2.add(a[i]);
      }
    }
    while (m > 0 && q1.size() >= 2) {
      long x = q1.poll();
      long y = q1.poll();
      q2.add(x + y);
      m--;
    }
    while (m > 0 && !q1.isEmpty()) {
      long x = q2.poll();
      long y = q1.poll();
      q2.add(x + y);
      m--;
    }
    while (m > 0 && q2.size() >= 2) {
      long x = q2.poll();
      long y = q2.poll();
      q2.add(x + y);
      m--;
    }
    long ans = 0;
    while (!q2.isEmpty()) {
      long x = q2.poll();
      if (x >= k) {
        ans += x - half;
      }
    }
    return ans;
  }

  public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    int n = scanner.nextInt();
    int m = scanner.nextInt();
    int k = scanner.nextInt();
    long[] a = new long[n];
    for (int i = 0; i < n; i++) {
      a[i] = scanner.nextLong();
    }
    System.out.println(Solution.solve(n, m, k, a));
    scanner.close();
  }
}
```
{% endtab %}
{% tab 2EGreedy Go %}
```go
package main

import (
	"bufio"
	"container/heap"
	"fmt"
	"os"
)

type MaxHeap []int64

func (h MaxHeap) Len() int { return len(h) }

func (h MaxHeap) Less(i, j int) bool { return h[i] > h[j] }

func (h MaxHeap) Swap(i, j int) { h[i], h[j] = h[j], h[i] }

func (h *MaxHeap) Push(x interface{}) {
	*h = append(*h, x.(int64))
}

func (h *MaxHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

func solve(n int, m int, k int, a []int64) int64 {
	q1 := &MaxHeap{}
	q2 := &MaxHeap{}
	heap.Init(q1)
	heap.Init(q2)
	half := (k + 1) / 2
	for _, x := range a {
		if x < int64(k) && x >= int64(half) {
			heap.Push(q1, x)
		} else {
			heap.Push(q2, x)
		}
	}

	for m > 0 && q1.Len() >= 2 {
		x := heap.Pop(q1).(int64)
		y := heap.Pop(q1).(int64)
		heap.Push(q2, x+y)
		m--
	}

	for m > 0 && q1.Len() > 0 {
		x := heap.Pop(q2).(int64)
		y := heap.Pop(q1).(int64)
		heap.Push(q2, x+y)
		m--
	}

	for m > 0 && q2.Len() >= 2 {
		x := heap.Pop(q2).(int64)
		y := heap.Pop(q2).(int64)
		heap.Push(q2, x+y)
		m--
	}

	ans := int64(0)
	for q2.Len() > 0 {
		x := heap.Pop(q2).(int64)
		if x >= int64(k) {
			ans += x - int64(half)
		}
	}

	return ans
}

func main() {
	var n, m, k int
	reader := bufio.NewReader(os.Stdin)

	fmt.Fscan(reader, &n, &m, &k)

	a := make([]int64, n)
	for i := 0; i < n; i++ {
		fmt.Fscan(reader, &a[i])
	}

	fmt.Println(solve(n, m, k, a))
}
```
{% endtab %}
{% endtabs %}
