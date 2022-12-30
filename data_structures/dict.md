---
layout: default
parent: データ構造
summary: 
---

# 辞書型 dict

python3.6 以降は辞書内の順序が保存されるようになった

## 使い方

```python
d = {}  # 空の辞書
d = {"apple": 3, "banana": 5, "orange": 8}  # 中身入りの辞書
```

## 計算量

|操作|コード|最悪計算量|備考|
|---|:-:|:-:|---|---|
|要素数を取得|`len(d)`|$O(1)$|`(key, value)` の組数|
|要素 `(key, value)` を追加| `d[key] = value` <br> `d.setdefault(key, value)` |$O(1)$|すでにキー `key` が存在するとき<br>`d[key] = value` は上書き<br>`d.setdefault(key, value)` は上書きしない|
|キー `key` を指定して値にアクセス|`d[key]` <br> `d.get(key)`|$O(1)$|`key` が存在しないとき<br>`d[key]` はエラー<br>`d.get(key)` は `None` を返す|
|辞書内にキー `key` があるか判定|`key in d` <br> `key in d.keys()`|$O(1)$|ハッシュ関数で計算するだけで判定できるため|
|辞書内に値 `value` があるか判定|`value in d.values()`|$O(n)$|
|キー `key` を指定して要素を削除|`d.pop(key)`|$O(1)$|返り値は `value`|


## Counter

- [PythonのCounterでリストの各要素の出現個数をカウント](https://note.nkmk.me/python-collections-counter/)

```python
from collections import Counter

l = ["a", "a", "a", "a", "b", "c", "c"]
c = Counter(l)  # リストやタプルを渡して Counter オブジェクトを生成
print(c)  # Counter({'a': 4, 'c': 2, 'b': 1})
```

## Union-Find

- [PythonでのUnion-Find（素集合データ構造）の実装と使い方](https://note.nkmk.me/python-union-find/)

<details markdown="1">
<summary>要素数を指定する実装</summary>

```python
# https://note.nkmk.me/python-union-find/
from collections import defaultdict

class UnionFind():
    def __init__(self, n) -> None:
        self.n = n
        # store (number of elements in group)*(-1) if element is a root
        self.parents = [-1]*n

    def union(self, x: int, y: int) -> None:
        "要素 x が属するグループと要素 y が属するグループとを併合する / O(α(n))"
        x = self.find(x)
        y = self.find(y)
        if x != y:
            if self.parents[x] > self.parents[y]:
                x, y = y, x
            self.parents[x] += self.parents[y]
            self.parents[y] = x

    def find(self, x: int) -> int:
        "要素 x が属するグループの根を返す / O(log n)"
        if self.parents[x] < 0:
            return x
        else:
            # path compression
            self.parents[x] = self.find(self.parents[x])
            return self.parents[x]

    def same(self, x: int, y: int) -> bool:
        "要素 x と要素 y が同じグループに属するかどうかを返す / O(log n)"
        return self.find(x) == self.find(y)

    def size(self, x: int) -> int:
        "要素 x が属するグループの要素数を返す / O(log n)"
        return self.parents[self.find(x)]

    def members(self, x: int) -> list:
        "要素 x が属するグループの要素をリストで返す / O(n log n)"
        root = self.find(x)
        return [i for i in range(self.n) if self.find(i) == root]

    def roots(self) -> list:
        "すべての根の要素をリストで返す / O(n)"
        return [i for i, x in enumerate(self.parents) if x < 0]

    def group_count(self) -> int:
        "グループの数を返す / O(n)"
        return len(self.roots())

    def all_group_members(self) -> defaultdict:
        "{ルート要素: [そのグループに含まれる要素のリスト], ...} の defaultdict を返す"
        group_members = defaultdict(list)
        for member in range(self.n):
            group_members[self.find(member)].append(member)
        return group_members

    def __str__(self) -> str:
        return "\n".join(f"{r}: {m}" for r, m in self.all_group_members().items())
```

</details>

<details>
<summary>UFdict の実装</summary>

```python
from collections import defaultdict

class UnionFind():
    def __init__(self, elements=None) -> None:
        class KeyDict(dict):
            def __missing__(self, key):
                self[key] = key
                return key
        self._parent = KeyDict()
        self._size = defaultdict(lambda: 1)

        if elements is not None:
            for element in elements:
                _, _ = self._parent[element], self._size[element]

    def find(self, x):
        if self._parent[x] == x:
            return x
        else:
            self._parent[x] = self.find(self._parent[x])
            return self._parent[x]

    def union(self, x, y) -> None:
        x = self.find(x)
        y = self.find(y)

        if x == y:
            return
        if self._size[x] < self._size[y]:
            x, y = y, x
        self._size[x] += self._size[y]
        self._parent[y] = x

    def same(self, x, y) -> bool:
        return self.find(x) == self.find(y)

    def size(self, x) -> int:
        return self._size[self.find(x)]

    def members(self, x) -> list:
        root = self.find(x)
        return [p for p in self._parent if self.find(p) == root]

    def roots(self) -> list:
        return [p for p, q in self._parent.items() if p == q]

    def group_count(self) -> int:
        return len(self.roots())

    def all_group_members(self) -> defaultdict:
        group_members = defaultdict(list)
        for member in self._parent:
            group_members[self.find(member)].append(member)
        return group_members

    def __str__(self) -> str:
        return "\n".join(f"{r}: {m}" for r, m in self.all_group_members().items())
```

</details>

```python
uf = UnionFind(10)  # 要素数 10 の Union-Find
uf = UnionFind()    # UFdict の場合は要素数を指定しない
```

|操作|コード|償却計算量|備考|
|---|:-:|:-:|---|---|
|`x` が属するグループと<br>`y` が属するグループとを併合|`uf.union(x, y)`|$O(\alpha(n))$|$\alpha(n)$ はアッカーマン関数の逆関数<br>ほぼ $O(1)$ とみなせる|
|`x` が属するグループの根を取得|`uf.find(x)`|$O(\log n)$||
|`x` と `y` が同じグループに属するか判定|`uf.same(x, y)`|$O(\log n)$|
|`x` が属するグループの要素数を取得|`uf.size(x)`|$O(\log n)$|

- グループの判定と併合が得意、グループの分割はできない
- 内部でデータを dict で保存する場合は、要素数を指定せずに使える