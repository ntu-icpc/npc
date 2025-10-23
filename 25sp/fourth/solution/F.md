---
title: F - Subsequence
layout: page
parent: NPC25 The 4th Contest Solution
---

# F - Subsequence

## Subtask 2 (Brute Force)
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  int n;
  long long x;
  cin >> n >> x;
  vector<long long> a(n);
  for (auto& i : a) cin >> i;
  int ans = 0;
  for (int msk = 1; msk < (1 << n); msk++) {
    int first = 1;
    int last = 0;
    bool can = true;
    for (int i = 0; i < n; i++) {
      if (msk & (1 << i)) {
        if (first) {
          first = 0;
          last = a[i];
        } else if (last * a[i] > x) {
          can = false;
          break;
        } else {
          last = a[i];
        }
      }
    }
    if (can) ans = max(ans, __builtin_popcount(msk));
  }
  cout << ans << "\n";
}
```
## Subtask 3 (n^2 DP)

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using vl = vector<ll>;

int main() {
  ll n, x;
  cin >> n >> x;
  vl a(n);
  for (auto& i : a) cin >> i;

  int dp[n];
  dp[0] = 1;
  int ans = 1;
  for (int i = 1; i < n; i++) {
    dp[i] = 1;
    for (int j = i - 1; j >= 0; j--) {
      if (a[i] * a[j] <= x) {
        dp[i] = max(dp[i], dp[j] + 1);
      }
    }
    ans = max(ans, dp[i]);
  }
  cout << ans << "\n";
}
```

## Solution (n log n DP optimization with Segment Tree)
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using vl = vector<ll>;

const int MAXN = 1e6;
int n, t[4 * MAXN];

int q(int v, int tl, int tr, int l, int r) {
  if (l > r) return 0;
  if (l == tl && r == tr) {
    return t[v];
  }
  int tm = (tl + tr) / 2;
  return max(q(v * 2, tl, tm, l, min(r, tm)),
             q(v * 2 + 1, tm + 1, tr, max(l, tm + 1), r));
}

void update(int v, int tl, int tr, int pos, int new_val) {
  if (tl == tr) {
    t[v] = new_val;
  } else {
    int tm = (tl + tr) / 2;
    if (pos <= tm)
      update(v * 2, tl, tm, pos, new_val);
    else
      update(v * 2 + 1, tm + 1, tr, pos, new_val);
    t[v] = max(t[v * 2], t[v * 2 + 1]);
  }
}

int main() {
  ll x;
  cin >> n >> x;
  vl a(n);
  
  memset(t, 0, sizeof(t));
  for (auto& i : a) cin >> i;

  set<ll> b;
  for (ll v : a) b.emplace(v);

  unordered_map<int, int> mpp;
  int idx = 0;
  for (int v : b) {
    mpp[v] = idx;
    idx++;
  }

  int dp[n];
  dp[0] = 1;
  int ans = 1;
  update(1, 0, b.size() - 1, mpp[a[0]], 1);
  for (int i = 1; i < n; i++) {
    
    int maxv = 0;
    if (x >= 0 && a[i] > 0) {
      auto it = b.upper_bound(x / a[i]);
      if (it != b.begin()) {
        it--;
        int idx = mpp[*it];
        maxv = q(1, 0, b.size() - 1, 0, idx);
      }
    } else if (x >= 0 && a[i] == 0) {
      maxv = q(1, 0, b.size() - 1, 0, b.size() - 1);
    } else if (x >= 0 && a[i] < 0) {
      auto it = b.lower_bound(
          floor(1.0 * x / a[i])); 
      if (it != b.end()) {
        if ((*it) * a[i] > x) {
          it++;
        }
      }
      if (it != b.end()) {
        int idx = mpp[*it];
        maxv = q(1, 0, b.size() - 1, idx, b.size() - 1);
      }
    } else if (x < 0 && a[i] > 0) {
      auto it = b.upper_bound(floor(1.0 * x / a[i]));

      if (it != b.begin()) {
        it--;
     
        int idx = mpp[*it];
        maxv = q(1, 0, b.size() - 1, 0, idx);
      }
    } else if (x < 0 && a[i] < 0) {
      auto it = b.lower_bound(x / a[i]);
      if (it != b.end()) {
        if ((*it) * a[i] > x) {
          it++;
        }
      }
      if (it != b.end()) {
        int idx = mpp[*it];
        maxv = q(1, 0, b.size() - 1, idx, b.size() - 1);
      }
    }

    dp[i] = maxv + 1;

    ans = max(ans, dp[i]);
    update(1, 0, b.size() - 1, mpp[a[i]], dp[i]);
  }
  cout << ans << "\n";
}
```

## Solution (Greedy)
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ii = pair<ll, ll>;
using vl = vector<ll>;
const ll INF = 1e9;

bool large(ii left, ii right) {
  if (left.first > right.first)
    return true;
  else if (left.first < right.first)
    return false;
  else if (abs(left.second) > abs(right.second))
    return true;
  return false;
}

ii maxf(ii left, ii right) {
  if (large(left, right))
    return left;
  else
    return right;
}

int main() {
  ll n, x;
  cin >> n >> x;
  vl a(n);
  for (auto& i : a) cin >> i;

  int len = 1;
  if (x >= 0) {
    ll last = a[0];
    for (int i = 1; i < n; i++) {
      if (last * a[i] <= x) {
        len++;
        last = a[i];
      } else if (abs(a[i]) < abs(last)) {
        last = a[i];
      }
    }
  } else {
    ii dp[n + 1][2];
    dp[0][0] = {0, INF + 1};
    dp[0][1] = {0, -1 * INF - 1};
    for (int i = 0; i < n; i++) {
      dp[i + 1][0] = dp[i][0];
      dp[i + 1][1] = dp[i][1];

      if (a[i] > 0) {  // +ve
        dp[i + 1][0] = maxf(dp[i + 1][0], {dp[i][0].first, a[i]});
        if (dp[i + 1][0].first == 0) dp[i + 1][0] = {1, a[i]};
        if (dp[i][1].second * a[i] <= x) {
          dp[i + 1][0] = maxf(dp[i + 1][0], {dp[i][1].first + 1, a[i]});
        }
      } else if (a[i] < 0) {  // -ve
        dp[i + 1][1] = maxf(dp[i + 1][1], {dp[i][1].first, a[i]});
        if (dp[i + 1][1].first == 0) dp[i + 1][1] = {1, a[i]};
        if (dp[i][0].second * a[i] <= x) {
          dp[i + 1][1] = maxf(dp[i + 1][1], {dp[i][0].first + 1, a[i]});
        }
      }
      // cout << i << " " << a[i] << " " << max(dp[i + 1][0].first , dp[i +
      // 1][1].first) << "\n";
      //  cout << i << ": \n";
      //  cout << dp[i + 1][0].first << " " << dp[i + 1][0].second << "\n";
      //  cout << dp[i + 1][1].first << " " << dp[i + 1][1].second << "\n";
    }
    len = max(dp[n][0].first, dp[n][1].first);
  }
  if (len == 0) len = 1;
  cout << len << "\n";
}
```