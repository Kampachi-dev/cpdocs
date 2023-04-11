---
layout: default
parent: データ構造
summary:
last_modified_date: 2022-12-31
---

# 集合型 set

```python
s = set()  # 空集合
s = {0, 1, 1, 2, 2, 2}  # 重複が取り除かれて {0, 1, 2} になる
```

| 操作                           |              コード               |      最悪計算量      | 備考                                                                                |
| ------------------------------ | :-------------------------------: | :------------------: | ----------------------------------------------------------------------------------- |
| 要素 `a` を追加                |            `s.add(a)`             |        $O(1)$        |
| 要素数を取得                   |             `len(s)`              |        $O(1)$        |
| 集合内に要素 `a` があるか判定  |             `a in s`              |        $O(1)$        | ハッシュ値で調べるため                                                              |
| 集合 `s` と集合 `t` との和集合 |                `s                 |  t`<br>`s.union(t)`  | $O(len(s) + len(t))$                                                                |
| 集合 `s` と集合 `t` との積集合 | `s & t` <br> `s.intersection(t)`  | $O(len(s) + len(t))$ |
| 要素 `a` を削除                | `s.remove(a)` <br> `s.discard(a)` |        $O(1)$        | 存在しない値を指定したとき<br>`s.remove(a)` はエラー<br>`s.discard(a)` は何もしない |

- `set` の `set` をとるときは `frozenset` を使う

## 順序付き集合 SortedSet / SortedMultiset

- tatyam 氏が平方分割による順序付き集合を実装している
  - [Python で std::set の代替物を作った - Qiita](https://qiita.com/tatyam/items/492c70ac4c955c055602)

<details markdown="1">
<summary>SortedSet の実装</summary>

```python
import math
from bisect import bisect_left, bisect_right
from typing import Generic, Iterable, Iterator, TypeVar, Union
T = TypeVar('T', int, float, str)


class SortedSet(Generic[T]):
    BUCKET_RATIO = 50
    REBUILD_RATIO = 170

    def _build(self, a=None) -> None:
        "Evenly divide `a` into buckets."
        if a is None:
            a = list(self)
        size = self.size = len(a)
        bucket_size = int(math.ceil(math.sqrt(size / self.BUCKET_RATIO)))
        self.a = [a[(size * i // bucket_size):(size * (i + 1) // bucket_size)]
                  for i in range(bucket_size)]

    def __init__(self, a: Iterable[T] = []) -> None:
        "Make a new SortedSet from iterable. / O(N) if sorted and unique / O(N log N)"
        a = list(a)
        if not all(a[i] < a[i + 1] for i in range(len(a) - 1)):
            a = sorted(set(a))
        self._build(a)

    def __iter__(self) -> Iterator[T]:
        for i in self.a:
            for j in i:
                yield j

    def __reversed__(self) -> Iterator[T]:
        for i in reversed(self.a):
            for j in reversed(i):
                yield j

    def __len__(self) -> int:
        return self.size

    def __repr__(self) -> str:
        return "SortedSet" + str(self.a)

    def __str__(self) -> str:
        s = str(list(self))
        return "{" + s[1: len(s) - 1] + "}"

    def _find_bucket(self, x: T) -> "list[T]":
        "Find the bucket which should contain x. self must not be empty."
        for a in self.a:
            if x <= a[-1]:
                return a
        return self.a[-1]

    def __contains__(self, x: T) -> bool:
        if self.size == 0:
            return False
        a = self._find_bucket(x)
        i = bisect_left(a, x)
        return i != len(a) and a[i] == x

    def add(self, x: T) -> bool:
        "Add an element and return True if added. / O(√N)"
        if self.size == 0:
            self.a = [[x]]
            self.size = 1
            return True
        a = self._find_bucket(x)
        i = bisect_left(a, x)
        if i != len(a) and a[i] == x:
            return False
        a.insert(i, x)
        self.size += 1
        if len(a) > len(self.a) * self.REBUILD_RATIO:
            self._build()
        return True

    def discard(self, x: T) -> bool:
        "Remove an element and return True if removed. / O(√N)"
        if self.size == 0:
            return False
        a = self._find_bucket(x)
        i = bisect_left(a, x)
        if i == len(a) or a[i] != x:
            return False
        a.pop(i)
        self.size -= 1
        if len(a) == 0:
            self._build()
        return True

    def lt(self, x: T) -> Union[T, None]:
        "Find the largest element < x, or None if it doesn't exist."
        for a in reversed(self.a):
            if a[0] < x:
                return a[bisect_left(a, x) - 1]

    def le(self, x: T) -> Union[T, None]:
        "Find the largest element <= x, or None if it doesn't exist."
        for a in reversed(self.a):
            if a[0] <= x:
                return a[bisect_right(a, x) - 1]

    def gt(self, x: T) -> Union[T, None]:
        "Find the smallest element > x, or None if it doesn't exist."
        for a in self.a:
            if a[-1] > x:
                return a[bisect_right(a, x)]

    def ge(self, x: T) -> Union[T, None]:
        "Find the smallest element >= x, or None if it doesn't exist."
        for a in self.a:
            if a[-1] >= x:
                return a[bisect_left(a, x)]

    def __getitem__(self, x: int) -> T:
        "Return the x-th element, or IndexError if it doesn't exist."
        if x < 0:
            x += self.size
        if x < 0:
            raise IndexError
        for a in self.a:
            if x < len(a):
                return a[x]
            x -= len(a)
        raise IndexError

    def index(self, x: T) -> int:
        "Count the number of elements < x."
        ans = 0
        for a in self.a:
            if a[-1] >= x:
                return ans + bisect_left(a, x)
            ans += len(a)
        return ans

    def index_right(self, x: T) -> int:
        "Count the number of elements <= x."
        ans = 0
        for a in self.a:
            if a[-1] > x:
                return ans + bisect_right(a, x)
            ans += len(a)
        return ans
```

</details>

<details markdown="1">
<summary>SortedMultiset の実装</summary>

```python
import math
from bisect import bisect_left, bisect_right, insort
from typing import Generic, Iterable, Iterator, TypeVar, Union
T = TypeVar('T', int, float, str)

class SortedMultiset(Generic[T]):
    BUCKET_RATIO = 50
    REBUILD_RATIO = 170

    def _build(self, a=None) -> None:
        "Evenly divide `a` into buckets."
        if a is None:
            a = list(self)
        size = self.size = len(a)
        bucket_size = int(math.ceil(math.sqrt(size / self.BUCKET_RATIO)))
        self.a = [a[(size * i // bucket_size):(size * (i + 1) // bucket_size)]
                  for i in range(bucket_size)]

    def __init__(self, a: Iterable[T] = []) -> None:
        "Make a new SortedMultiset from iterable. / O(N) if sorted / O(N log N)"
        a = list(a)
        if not all(a[i] <= a[i + 1] for i in range(len(a) - 1)):
            a = sorted(a)
        self._build(a)

    def __iter__(self) -> Iterator[T]:
        for i in self.a:
            for j in i:
                yield j

    def __reversed__(self) -> Iterator[T]:
        for i in reversed(self.a):
            for j in reversed(i):
                yield j

    def __len__(self) -> int:
        return self.size

    def __repr__(self) -> str:
        return "SortedMultiset" + str(self.a)

    def __str__(self) -> str:
        s = str(list(self))
        return "{" + s[1: len(s) - 1] + "}"

    def _find_bucket(self, x: T) -> "list[T]":
        "Find the bucket which should contain x. self must not be empty."
        for a in self.a:
            if x <= a[-1]:
                return a
        return self.a[-1]

    def __contains__(self, x: T) -> bool:
        if self.size == 0:
            return False
        a = self._find_bucket(x)
        i = bisect_left(a, x)
        return i != len(a) and a[i] == x

    def count(self, x: T) -> int:
        "Count the number of x."
        return self.index_right(x) - self.index(x)

    def add(self, x: T) -> None:
        "Add an element. / O(√N)"
        if self.size == 0:
            self.a = [[x]]
            self.size = 1
            return
        a = self._find_bucket(x)
        insort(a, x)
        self.size += 1
        if len(a) > len(self.a) * self.REBUILD_RATIO:
            self._build()

    def discard(self, x: T) -> bool:
        "Remove an element and return True if removed. / O(√N)"
        if self.size == 0:
            return False
        a = self._find_bucket(x)
        i = bisect_left(a, x)
        if i == len(a) or a[i] != x:
            return False
        a.pop(i)
        self.size -= 1
        if len(a) == 0:
            self._build()
        return True

    def lt(self, x: T) -> Union[T, None]:
        "Find the largest element < x, or None if it doesn't exist."
        for a in reversed(self.a):
            if a[0] < x:
                return a[bisect_left(a, x) - 1]

    def le(self, x: T) -> Union[T, None]:
        "Find the largest element <= x, or None if it doesn't exist."
        for a in reversed(self.a):
            if a[0] <= x:
                return a[bisect_right(a, x) - 1]

    def gt(self, x: T) -> Union[T, None]:
        "Find the smallest element > x, or None if it doesn't exist."
        for a in self.a:
            if a[-1] > x:
                return a[bisect_right(a, x)]

    def ge(self, x: T) -> Union[T, None]:
        "Find the smallest element >= x, or None if it doesn't exist."
        for a in self.a:
            if a[-1] >= x:
                return a[bisect_left(a, x)]

    def __getitem__(self, x: int) -> T:
        "Return the x-th element, or IndexError if it doesn't exist."
        if x < 0:
            x += self.size
        if x < 0:
            raise IndexError
        for a in self.a:
            if x < len(a):
                return a[x]
            x -= len(a)
        raise IndexError

    def index(self, x: T) -> int:
        "Count the number of elements < x."
        ans = 0
        for a in self.a:
            if a[-1] >= x:
                return ans + bisect_left(a, x)
            ans += len(a)
        return ans

    def index_right(self, x: T) -> int:
        "Count the number of elements <= x."
        ans = 0
        for a in self.a:
            if a[-1] > x:
                return ans + bisect_right(a, x)
            ans += len(a)
        return ans
```

</details>

```python
s = SortedSet([])  # 空のリストを渡して SortedSet オブジェクトを生成 (SortedMultiset も同様)
```

| 操作                                |       コード       |  償却計算量   | 備考                                                            |
| ----------------------------------- | :----------------: | :-----------: | --------------------------------------------------------------- |
| 要素数を取得                        |      `len(s)`      |    $O(1)$     |
| 要素 `a` を追加                     |     `s.add(a)`     | $O(\sqrt{n})$ | `SortedSet` は重複を許さない <br> `SortedMultiset` は重複を許す |
| 要素 `a` を削除                     |   `s.discard(a)`   | $O(\sqrt{n})$ | `SortedMultiset` では 1 個だけ削除                              |
| `a` より小さい最大の要素を取得      |     `s.lt(a)`      | $O(\sqrt{n})$ | less than                                                       |
| `a` 以下で最大の要素を取得          |     `s.le(a)`      | $O(\sqrt{n})$ | less than or equal                                              |
| `a` より大きい最小の要素を取得      |     `s.gt(a)`      | $O(\sqrt{n})$ | greater than                                                    |
| `a` 以上で最小の要素を取得          |     `s.ge(a)`      | $O(\sqrt{n})$ | greater than or equal                                           |
| 小さいほうから `k` 番目の要素を取得 |       `s[k]`       | $O(\sqrt{n})$ |
| `k` より小さい要素の数を取得        |    `s.index(k)`    | $O(\sqrt{n})$ |
| `k` 以下の要素の数を取得            | `s.index_right(k)` | $O(\sqrt{n})$ |

- 償却計算量について
  > 平衡二分探索木は各操作が $O(\log N)$ 、平方分割は各操作 $O(\sqrt{N})$ であるため、平衡二分探索木の方が速そうに見えますが、うまく高速化すれば、(競プロ実用の範囲では) 平衡二分探索木と同等かそれ以上の速さを得ることができます。
  > これには、Python の言語特性だったり、list という組み込み型を使えることが影響していそうです。
