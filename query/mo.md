---
layout: default
parent: クエリ処理
summary: 
mermaid: true
last_modified_date:
---

# Mo's algorithm

- [Mo's algorithm - ei1333の日記](https://ei1333.hateblo.jp/entry/2017/09/11/211011)
- [Mo's algorithm(クエリ平方分割)の話 - Qiita](https://qiita.com/ageprocpp/items/34121c58e571ea8c4023)

区間に関するクエリを先読みして適切な順番に並び替え、高速に処理するためのアルゴリズム。幅 $N$ の数列に対してクエリが $Q$ 個あるとき、端を 1 マス伸縮させるときの計算量を $O(\alpha)$ とすると、クエリ全体の計算量は $O(\alpha N \sqrt{Q})$ となる。

`_init_states()` の初期状態定義と `_add()` / `_delete()` の処理、`process()` 内のクエリを処理して結果を格納する部分は、その都度自分で書き換える。

- [ABC242G - Range Pairing Query](https://atcoder.jp/contests/abc242/tasks/abc242_g)

```python
from math import ceil, sqrt

class Mo:
    def __init__(self, L):
        self.L = L
        self.N = len(L)

    def _init_states(self):
        # 解くのに必要な状態を定義する
        self.pairs = 0
        self.count = [0] * (N+1)
        # [l, r) の半開区間で考える
        self.l = 0
        self.r = 0

    def _add(self, i):
        "区間の端に i を追加するときの処理"
        self.count[self.L[i]] += 1
        if self.count[self.L[i]] % 2 == 0:
            self.pairs += 1

    def _delete(self, i):
        "区間の端から i を削除するときの処理"
        self.count[self.L[i]] -= 1
        if self.count[self.L[i]] % 2 == 1:
            self.pairs -= 1

    def _one_process(self, l, r):
        "クエリのために区間を伸縮させる"
        while l < self.l:
            self.l -= 1
            self._add(self.l)
        while self.r < r:
            self.r += 1
            self._add(self.r-1)
        while self.l < l:
            self._delete(self.l)
            self.l += 1
        while r < self.r:
            self._delete(self.r-1)
            self.r -= 1

    def process(self, queries):
        self._init_states()
        self.b = ceil(sqrt(len(queries)))
        self.buckets = [list() for b in range(self.b)]

        q = []
        for i in range(len(queries)):
            l, r = queries[i]
            q.append(l * 10**12 + r * 10**6 + i)
        q.sort()

        for i in range(len(queries)):
            self.buckets[i // self.b].append(q[i])

        ans = [-1] * len(queries)
        for i in range(self.b):
            b = []
            for query in self.buckets[i]:
                l = query // 10**12
                r = (query % 10**12) // 10**6
                i = query % 10**6
                b.append(r * 10**12 + l * 10**6 + i)
            if i % 2 == 0:
                b.sort()
            else:
                b.sort(reverse=True)
            for query in b:
                l = (query % 10**12) // 10**6
                r = query // 10**12
                i = query % 10**6

                # ここでクエリを処理して結果を格納する
                self._one_process(l, r)
                ans[i] = self.pairs

        return ans
```

```python
# L: 対象となる配列
# queries: 半開区間 [l, r) をタプル (l, r) にして配列に格納したもの
mo = Mo(L)
ans = mo.process(queries)
for q in range(Q):
    print(ans[q])
```