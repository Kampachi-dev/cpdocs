---
layout: default
parent: グラフアルゴリズム
summary: 
last_modified_date: 2022-12-31
---

# トポロジカルソート

有向非巡回グラフ (DAG) をトポロジカルソートする。

入次数 `indegree` を管理する。グラフの辺を順に削除していき、入次数が 0 になったものを順に並べるとトポロジカルソートが実現できる。

キューの管理を優先度付きキューにすれば辞書順最小になる。

- [ABC223-D Restricted Permutation](https://atcoder.jp/contests/abc223/tasks/abc223_d)

```python
indegree = [0] * N
graph = [set() for n in range(N)]

que = []
order = []
while que:
    u = que.pop()
    order.append(u)
    for v in graph[u]:
        indegree[v] -= 1
        if indegree[v] == 0:
            que.append(v)

if len(order) == N:
    # トポロジカルソート成立
else:
    # トポロジカルソート不成立 (グラフにサイクルが存在する)
```