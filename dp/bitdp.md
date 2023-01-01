---
layout: default
parent: 動的計画法
summary: 
last_modified_date: 2022-12-31
---

# bitDP

- [鉄則本 B23](https://atcoder.jp/contests/tessoku-book/tasks/tessoku_book_cv)
- [ビットDP(bit DP)の考え方 ～集合に対する動的計画法～](https://algo-logic.info/bit-dp/)

$N$ 個の都市を順番に巡るときの最短距離を求める
訪問した都市の集合と現在位置を持って DP をする。

```
dp[s][i]: すでに訪問した都市の集合が s で、現在位置が i であるときの最短移動距離
```

を考える。例として、$N=4$ で都市 $0$ からすべての都市を周って戻ってくることを考える。`dist(a->b)` を `a` から `b` までの移動距離とすると、

```
dp[{0, 1, 2, 3}][0] = min(dp[{1, 2, 3}][1] + dist(1->0),
                          dp[{1, 2, 3}][2] + dist(2->0),
                          dp[{1, 2, 3}][3] + dist(3->0))
```

と書ける。そのうち都市 $2$ を選ぶと再帰的に

```
dp[{1, 2, 3}][2] = min(dp[{1, 3}][1] + dist(1->2),
                       dp[{1, 3}][3] + dist(3->2))
```

となる。これを書き下してメモ化再帰すると、遷移を考えなくてよくなる。

```python
def dfs(s: int, i: int):
    if dp[s][i] < max_d:
        return dp[s][i]
    if s == 2**i:
        if dp[s][i] == max_d:
            dp[s][i] = dist(0, i)
        return dp[s][i]
    for k in range(N):
        # 集合 s に k が含まれているか判定しておく
        if s & 2**k and i != k:
            dp[s][i] = min(dp[s][i], dfs(s-2**i, k) + dist(k, i))
    return dp[s][i]

dfs(2**N-1, 0)
print(dp[2**N-1][0])
```

また、2 進数の数 $i$, $j$ が表す集合が $S(i) \subset S(j)$ ならば $i \leq j$ となる性質から、単純な 2 重ループで書いてもよい。

```python
# 今いる都市が i で次に行く都市が j
for s in range(2**N):
    for i in range(N):
        for j in range(N):
            # 都市 j に行ったことがなければ計算する
            if s // (2 ** j) % 2 == 0:
                dp[s+2**j][j] = min(dp[s+2**j][j], dp[s][i] + dist(i, j))

print(dp[2**N-1][0])
```