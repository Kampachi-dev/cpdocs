---
layout: default
parent: 動的計画法
summary: 
last_modified_date: 2022-12-31
---

# 最長増加部分列

最長増加部分列 (Longest Increasing Subsequence, LIS) の長さを求める。

<details markdown="1">
<summary>部分列とは</summary>

数列 $A$ に対して、

- $A$ を構成する要素を $0$ 個以上取り除き
- 残った要素を元の順番で並べる

ことによって得られる数列を $A$ の部分列という。

</details>

- [最長増加部分列(LIS)の長さを求める - Qiita](https://qiita.com/python_walker/items/d1e2be789f6e7a0851e5)
- [A24 LIS](https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_x)

増加部分列のうち、どの隣接する要素 $A_i$, $A_{i+1}$ についても $A_i \leq A_{i+1}$ を許すものを広義増加部分列、許さないものを狭義増加部分列という。

```python
from bisect import bisect_left, bisect_right

N = int(input())
A = [0] + list(map(int, input().split()))

# dp[i]: 最後の要素が A[i] である増加部分列のうち最長のものの長さ
dp = [0] * (N+1)

# L[x]: 長さ x の増加部分列の最後の要素として考えられる最小値
L = [INF] * (N+1)

dp[1] = 1
L[0] = 0
L[1] = A[1]

for i in range(2, N+1):
    # 狭義増加部分列なら bisect_left
    # 広義増加部分列なら bisect_right
    b = bisect_left(L, A[i])
    L[b] = A[i]
    dp[i] = b

print(max(dp))
```