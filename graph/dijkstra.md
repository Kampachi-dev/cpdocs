---
layout: default
parent: グラフアルゴリズム
summary: 
last_modified_date: 2023-01-22
---

# ダイクストラ法

単一始点最短経路問題 (始点を固定した時に、他のすべての頂点への最短経路を求める問題) を効率的に解くアルゴリズム。

- [ダイクストラ法による単一始点最短経路を求めるアルゴリズム](https://algo-logic.info/dijkstra/)

頂点 $E$ 個、辺 $V$ 本のグラフにダイクストラ法を適用すると、優先度付きキューを利用することで、計算量は $O(E \log V)$ となる。

```python
from heapq import heapify, heappop, heappush

N, M = map(int, input().split())
graph = [dict() for n in range(N+1)]
for m in range(M):
    A, B, C = map(int, input().split())
    graph[A][B] = C
    graph[B][A] = C

INF = 10 ** 18
dist = [INF] * (N+1)
dist[1] = 0
que = [(dist[1], 1)]
heapify(que)

while que:
    d, u = heappop(que)
    if dist[u] != d:
        continue
    for v in graph[u]:
        if dist[v] > d + graph[u][v]:
            dist[v] = d + graph[u][v]
            heappush(que, (dist[v], v))
```