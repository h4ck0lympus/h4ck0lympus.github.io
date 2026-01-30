---
title: "CP notes"
date: 2026-01-19T18:28:12-07:00
draft: false
math: true
---

### Template

```cpp
#include "bits/stdc++.h"

using namespace std;

#define int long long
#define HAVE_TESTCASES 0

void solve() {

}

#undef int
int main() {
  ios_base::sync_with_stdio(false);
  cin.tie(0);
#if HAVE_TESTCASES
  int t;
  cin >> t;
  while (t--) solve();
#else
  solve();
#endif
}
```

A common rule of thumb:
- About **$10^8$ operations per second**

Rough feasibility guide ($n$ = input size):

| $n$ upper bound | Possible complexities |
|-----------------|----------------------|
| $10$           | $O(n!)$, $O(n^7)$, $O(n^6)$   |
| $20$           | $O(2^n \cdot n)$, $O(n^5)$      |
| $80$           | $O(n^4)$                 |
| $400$          | $O(n^3)$                 |
| $7{,}500$        | $O(n^2)$                 |
| $7 \cdot 10^4$      | $O(n \sqrt{n})$               |
| $5 \cdot 10^5$      | $O(n \log n)$            |
| $5 \cdot 10^6$      | $O(n)$                  |
| $10^{18}$         | $O(\log^2 n)$, $O(\log n)$, $O(1)$ |

## Fundamental data structures

1. `vector`
2. `multiset` stores elements in sorted order and allows duplicates.

  * Key properties:
  - Logarithmic insertion and deletion
  - Efficient access to smallest or largest elements
  - Supports queries such as “largest value $\leq X$”

  * Use a multiset when:
  - Order matters
  - Elements are added and removed dynamically
  - You need to repeatedly choose the best valid option

  * for `multiset<int> m`, `m.begin()` will point to first element while, `prev(m.end())` will point to last element. 


## Intutional problems

#### 1. Generating permuations based on relation between 2-3 elements in range of $N$

[2188A](https://codeforces.com/contest/2188/problem/A)

## Greedy algo
1. In greedy string problems, sometimes you have to make the choice out of 2 strings. For example, In lexicographical problems where you only have two choices per step, the greedy choice is almost always: "Pick the smaller of the two resulting strings." Rather than the old string. (see 2180B).

## Binary search on arrays and for answer
## Math
## String

#### Circular Problems & The Concat Trick
Concept: To handle circularity in arrays or strings, append the object to itself ($S' = S + S$).

Pros:

• Simplifies logic: Turns circular gaps/patterns into linear ones.

• Eliminates complex `if` statements and modulo arithmetic.

Implementation Tips:

• Limit the Range: Always ensure your results (like max gap length) do not exceed the original length $N$.

• Complexity: Watch for $O(2N)$ memory/time; usually fine for $N \le 10^5$.

Alternative (Index Method): Store indices of key elements (e.g., all 1s).

• Internal gaps: $pos[i+1] - pos[i] - 1$.

• Circular gap: $(N - 1 - pos.back()) + pos[0]$.

Relevant problems: 
1. [2176B](https://codeforces.com/problemset/problem/2176/B) (classic example)

### Array

#### Find median in sliding window in $O(n)$ time

## Misc coding tips

#### Directional array to reduce code

instead of doing the following and checking each co-ordinate seperately we can just use a loop - much simpler and shorter.

```cpp
 if (cr != 0 && mp[cr-1][cc] == '.')  // up
 
 if (cr != row - 1 && mp[cr+1][cc] == '.') // down
 
 if (cc != 0 && mp[cr][cc-1] == '.') // left
 
 if (cc != col - 1 && mp[cr][cc+1] == '.') // right
```

```

int dr[4] = {-1, +1, 0, 0};
int dc[4] = {0, 0, -1, +1};


for (int i=0; i < 4; i++) {
  int crr = cr + dr[i], ccc = cc + dc[i];
  if (crr >= 0 && crr < row && ccc >= 0 && ccc < col)  {
    // do stuff  
  }
}
```

#### Todo
1. Modular arithmetic
