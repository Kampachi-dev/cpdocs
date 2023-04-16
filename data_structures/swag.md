---
layout: default
parent: データ構造
summary:
last_modified_date:
---

# スライド最小値

- [Foldable-Queue(SWAG) およびその Deque 版について - Qiita](https://qiita.com/Shirotsume/items/4a2837b5895ef9a7aeb1)
- [ABC298D - Writing a Numeral](https://atcoder.jp/contests/abc298/tasks/abc298_d)

両端キューに加えて、添字アクセスや列全体の総積を $O(1)$ で処理できるデータ構造。stack を 2 本組み合わせて構成されている。

<details markdown="1">
<summary>FoldableDeque の実装</summary>

```python

from itertools import chain
from typing import Callable, Iterator


class FoldableDeque():
    def __init__(self, op: Callable, e, L: list = []):
        self._op = op
        self._e = e
        self._top = []
        self._bottom = L
        self._topfold = [e]
        self._bottomfold = [e]
        for n in range(len(L)):
            self._bottomfold.append(self._op(self._bottomfold[-1], self._bottom[n]))

    def _append_bottom(self, x) -> None:
        self._bottom.append(x)
        self._bottomfold.append(self._op(self._bottomfold[-1], x))

    def _pop_bottom(self):
        self._bottomfold.pop()
        return self._bottom.pop()

    def _append_top(self, x) -> None:
        self._top.append(x)
        self._topfold.append(self._op(x, self._topfold[-1]))

    def _pop_top(self):
        self._topfold.pop()
        return self._top.pop()

    def append(self, x) -> None:
        self._append_bottom(x)

    def appendleft(self, x) -> None:
        self._append_top(x)

    def pop(self):
        if self._bottom:
            return self._pop_bottom()
        if not self._top:
            raise IndexError
        stack_top = []
        n = len(self._top)
        for _ in range(n//2):
            stack_top.append(self._pop_top())
        for _ in range(n//2, n):
            self._append_bottom(self._pop_top())
        for _ in range(n//2):
            self._append_top(stack_top.pop())
        return self._pop_bottom()

    def popleft(self):
        if self._top:
            return self._pop_top()
        if not self._bottom:
            raise IndexError
        stack_bottom = []
        n = len(self._bottom)
        for _ in range(n//2):
            stack_bottom.append(self._pop_bottom())
        for _ in range(n//2, n):
            self._append_top(self._pop_bottom())
        for _ in range(n//2):
            self._append_bottom(stack_bottom.pop())
        return self._pop_top()

    def fold(self):
        return self._op(self._topfold[-1], self._bottomfold[-1])

    def __len__(self) -> int:
        return len(self._top) + len(self._bottom)

    def __str__(self) -> str:
        s_top = ", ".join((str(x) for x in reversed(self._top)))
        s_bottom = ", ".join((str(x) for x in self._bottom))
        if s_top and s_bottom:
            return "[" + s_top + ", " + s_bottom + "]"
        else:
            return "[" + s_top + s_bottom + "]"

    def __iter__(self) -> Iterator:
        return chain(reversed(self._top), self._bottom)

    def __getitem__(self, i: int):
        if i < 0:
            i += len(self)
        if i < 0 or len(self) <= i:
            raise IndexError
        if 0 <= i < len(self._top):
            return self._top[~i]
        if len(self._top) <= i < len(self):
            return self._bottom[i - len(self._top)]
```

</details>

```python
que = FoldableDeque(op, e, L)
```

| 操作                      |       コード        | 償却計算量 | 備考                                   |
| ------------------------- | :-----------------: | :--------: | -------------------------------------- |
| **末尾に**要素 `a` を挿入 |   `que.append(a)`   |   $O(1)$   | `collections.deque` と同じように使える |
| **先頭に**要素 `a` を挿入 | `que.appendleft(a)` |   $O(1)$   |
| **末尾**の要素を削除      |     `que.pop()`     |   $O(1)$   |
| **先頭**の要素を削除      |   `que.popleft()`   |   $O(1)$   |
| 列の総積を求める          |    `que.fold()`     |   $O(1)$   |
| 要素数を取得              |      `len(s)`       |   $O(1)$   |
| $i$ 番目の要素にアクセス  |       `s[k]`        |   $O(1)$   |

- イテレータも実装してあるので、for 文などで回すこともできる
