---
layout: default
parent: グラフアルゴリズム
summary: 
last_modified_date:
---

# ワーシャルフロイド法

全点対最短経路問題 (すべての頂点に対して、他のすべての頂点への最短経路を求める問題) を効率的に解くアルゴリズム。

- [ワーシャル・フロイド法での全点対最短経路を求めるアルゴリズム](https://algo-logic.info/warshall-floyd/)

頂点 $u$ から頂点 $v$ に行くのに、頂点 $k$ を経由した方がよいかを調べる。頂点数 $N$ に対して計算量は $O(N^3)$ となる。

工夫すれば経路復元や負閉路検出もできる。

```python

INF = 10 ** 18
dist = [[INF]*N for n in range(N)]

for i in range(N):
    for j in range(N):
        if i == j:
            dist[i][j] = 0
        if # i -> j の辺があれば
            dist[i][j] = # 距離を設定

for k in range(N):
    for u in range(N):
        for v in range(N):
            dist[u][v] = min(dist[u][v], dist[u][k] + dist[k][v])
```