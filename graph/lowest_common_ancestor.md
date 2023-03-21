---
layout: default
parent: グラフアルゴリズム
summary:
last_modified_date:
---

# 最小共通祖先

根付き木の 2 頂点に対しての最小共通祖先 (lowest common ancestor, LCA) を求める。

- [最小共通祖先 [いかたこのたこつぼ]](https://ikatakos.com/pot/programming_algorithm/graph_theory/lowest_common_ancestor)
- [ABC148F - Playing Tag on Tree](https://atcoder.jp/contests/abc148/tasks/abc148_f)
- [ABC294G - Distance Queries on a Tree](https://atcoder.jp/contests/abc294/tasks/abc294_g)

## Euler Tour + Range Minimum Query

Euler Tour で区間を特定して、RmQ で区間内の最も浅い頂点を求める。

### Euler Tour

- [Euler Tour のお勉強 - maspy の HP](https://maspypy.com/euler-tour-%E3%81%AE%E3%81%8A%E5%8B%89%E5%BC%B7)
- [再帰を使わないオイラーツアーをスタックで実装する思考過程 - cocoinit23](https://cocoinit23.com/python-euler-tour-using-stack-not-recursive/)

木の頂点を DFS で通った順に並べる。pypy は再帰がとても遅いので、非再帰で書く。コードでは頂点 `U` を根としている。

```python
euler_tour = []
euler_tour_depth = []
depth = [-1] * (N+1)
depth[U] = 0

stack = [~U, U]
current_time = -1

parent = [-1] * (N+1)
in_time = [-1] * (N+1)
out_time = [-1] * (N+1)

while stack:
    u = stack.pop()
    current_time += 1
    if u >= 0:  # 行き
        euler_tour.append(u)
        euler_tour_depth.append(depth[u])
        if in_time[u] == -1:
            in_time[u] = current_time
        for v in graph[u]:
            if parent[u] != v:
                parent[v] = u
                stack.append(~v)
                stack.append(v)
                depth[v] = depth[u] + 1
    else:  # 帰り
        out_time[~u] = current_time
        if euler_tour[-1] != ~u:
            euler_tour.append(~u)
            euler_tour_depth.append(depth[~u])
        euler_tour.append(parent[~u])
        euler_tour_depth.append(depth[parent[~u]])
```

### Range Minimum Query

`euler_tour` の指定した区間に含まれる `euler_tour_depth` が一番小さい頂点を求めたい。区間クエリは[セグ木]({{ site.baseurl }}/data_structures/segment_tree)で解決。

深さと頂点との組をタプルとして持ってもいいが、タプルは遅いので整数に合成してあとから復元している。

```python
l = []
for i in range(len(euler_tour)):
    l.append(euler_tour_depth[i] * 10**9 + euler_tour[i])

segtree = SegmentTree(min, 10**18, l)
```

```python
res = segtree.query(l, r+1)
d = res // 10**9
v = res % 10**9
```
