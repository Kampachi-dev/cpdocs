---
layout: default
parent: データ構造
summary:
last_modified_date: 2023-12-16
---

# セグメントツリー

モノイドの区間演算を効率的に行うデータ構造。

<details markdown="1">
<summary>モノイドとは</summary>

以下の性質を満たす代数構造のこと。

- 単位元がある
  - ある元 $e$ が存在して、任意の元 $a$ に対して $$ea = ae = a$$ が成り立つとき、$e$ を単位元という
- 結合法則を満たす演算が定義されている
  - 任意の元 $a$, $b$, $c$ に対して $$(a \circ b) \circ c = a \circ (b \circ c)$$ が成り立つとき、結合法則を満たすという

</details>

| 求めたいもの       |                         関数 `op`                          | 単位元 `e` |
| ------------------ | :--------------------------------------------------------: | :--------: |
| 最小値             |                        `min(x, y)`                         |   `INF`    |
| 最大値             |                        `max(x, y)`                         |   `-INF`   |
| 区間和             |                        `add(x, y)`                         |    `0`     |
| 区間積             |                        `mul(x, y)`                         |    `1`     |
| 最大公約数         |                        `gcd(x, y)`                         |    `0`     |
| 排他的論理和       |                        `xor(x, y)`                         |    `0`     |
| ローリングハッシュ | (ハッシュ値, 文字列長) を 2 つ受け取って合体したものを返す |  `(0, 0)`  |

- ABC331F

<details markdown="1">
<summary>一点更新区間取得セグ木の実装</summary>

```python
from typing import Callable


class SegmentTree:
    def __init__(self, op: Callable, e, L):
        self.size = len(L)
        self.op = op
        self.e = e
        self.x = 0
        while 2 ** self.x < len(L):
            self.x += 1
        self.tree = [self.e] * (2 ** (self.x + 1))
        for i in range(len(L)):
            self.tree[i + (2 ** self.x)] = L[i]
        for i in range(2 ** self.x - 1, 0, -1):
            self.tree[i] = self.op(self.tree[2*i], self.tree[2*i+1])

    def update(self, i: int, x) -> None:
        "update the i-th element to x"
        i += 2 ** self.x
        self.tree[i] = x
        while i > 1:
            i //= 2
            self.tree[i] = self.op(self.tree[2*i], self.tree[2*i+1])

    def __setitem__(self, i: int, x) -> None:
        self.update(i, x)

    def get(self, i: int):
        "Return the i-th element, or IndexError if it doesn't exist"
        if i < 0:
            i += self.size
        if i < 0 or self.size <= i:
            raise IndexError
        return self.tree[i + 2 ** self.x]

    def __getitem__(self, i: int):
        return self.get(i)

    def query(self, l: int, r: int):
        "Return query in [l, r)"
        result_l = self.e
        result_r = self.e
        l += 2 ** self.x
        r += 2 ** self.x
        while l < r:
            if l % 2 == 1:
                result_l = self.op(result_l, self.tree[l])
                l += 1
            if r % 2 == 1:
                r -= 1
                result_r = self.op(self.tree[r], result_r)
            l //= 2
            r //= 2
        return self.op(result_l, result_r)
```

</details>

| 操作                                   |                   コード                   |   計算量    | 備考 |
| -------------------------------------- | :----------------------------------------: | :---------: | ---- |
| セグメントツリーを構築する             |     `segtree = SegmentTree(op, e, A)`      |   $O(n)$    |      |
| リストの $i$ 番目の値を取得する        |      `segtree.get(i)`<br>`segtree[i]`      |   $O(1)$    |      |
| リストの $i$ 番目の値を `x` に更新する | `segtree.update(i, x)`<br>`segtree[i] = x` | $O(\log n)$ |      |
| 半開区間 $[l, r)$ の集約値を求める     |           `segtree.query(l, r)`            | $O(\log n)$ |      |
