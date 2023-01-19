---
layout: default
parent: グラフアルゴリズム
summary: 
last_modified_date:
---

# 有向グラフのサイクル検出



## 有向グラフのサイクルに含まれる頂点を列挙する

- [ABC245F Endless Walk](https://atcoder.jp/contests/abc245/tasks/abc245_f)

有向グラフ内にサイクルが複数個含まれるとき、そのサイクルをすべて抜き出して含まれる頂点を調べる。

1. 以下のデータを用意する
    - `edges`: グラフの辺の集合
    - `graph`: 有向グラフ (隣接リスト)
    - `graph_back`: 辺の向きを逆にした有向グラフ (隣接リスト)
    - `indegree`: `graph` の各頂点の入次数
    - `outdegree`: `graph` の各頂点の出次数
2. BFS の要領で `outdegree[v] == 0` な頂点 `v` を `graph` から順次削除し、`v` に伸びる辺を `edges` から削除する
3. BFS の要領で `indegree[u] == 0` な頂点 `u` を `graph` から順次削除し、`u` から伸びる辺を `edges` から削除する
4. `edges` に残った辺はサイクルを構成するために必要な辺である

<details markdown="1">
<summary>実装</summary>

```python
from collections import deque

N, M = map(int, input().split())
edges = set()
for m in range(M):
    U, V = map(int, input().split())
    edges.add((U, V))

graph = [set() for n in range(N+1)]
outdegree = [0] * (N+1)
indegree = [0] * (N+1)
graph_back = [set() for n in range(N+1)]
for edge in edges:
    u, v = edge
    graph[u].add(v)
    outdegree[u] += 1
    indegree[v] += 1
    graph_back[v].add(u)

que = deque()
for n in range(N+1):
    if outdegree[n] == 0:
        que.append(n)

while que:
    v = que.popleft()
    for u in graph_back[v]:
        edges.remove((u, v))
        graph[u].remove(v)
        outdegree[u] -= 1
        indegree[v] -= 1
        if outdegree[u] == 0:
            que.append(u)

que = deque()
for n in range(N+1):
    if indegree[n] == 0:
        que.append(n)

while que:
    u = que.popleft()
    for v in graph[u]:
        edges.remove((u, v))
        outdegree[u] -= 1
        indegree[v] -= 1
        if indegree[v] == 0:
            que.append(v)
    graph[u] = set()


v_cycle = set()
for edge in edges:
    u, v = edge
    v_cycle.add(u)
    v_cycle.add(v)
```

</details>