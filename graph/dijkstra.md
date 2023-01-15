---
layout: default
parent: グラフアルゴリズム
summary: 
last_modified_date:
---

# ダイクストラ法

単一始点最短経路問題 (始点を固定した時に、他のすべての頂点への最短経路を求める問題) を効率的に解くアルゴリズム。

- [ダイクストラ法による単一始点最短経路を求めるアルゴリズム](https://algo-logic.info/dijkstra/)

頂点 $E$ 個、辺 $V$ 本のグラフにダイクストラ法を適用すると、優先度付きキューを利用することで、計算量は $O(E \log V)$ となる。

```python
from heapq import heapify, heappop, heappush

N, M = map(int, input().split())
graph = [set() for n in range(N)]
for m in range(M):
    A, B = map(int, input().split())
    graph[A].add(B)
    graph[B].add(A)

dist = [-1] * N
dist[0] = 0
que = [(dist[0], 0)]
heapify(que)

while que:
    d, u = heappop(que)
    if dist[u] != -1:
        continue
    dist[u] = d
    for v in graph[u]:
        if dist[v] == -1:
            heappush(que, (dist[v], v))

print(max(dist))
```