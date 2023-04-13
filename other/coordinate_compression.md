---
layout: default
parent: その他
summary:
last_modified_date:
---

# 座標圧縮

数列の各要素の大きさの**順番**だけが必要になることがある。このとき、

```
[10, 30, 20, 40, 30, 50]
```

を

```
[0, 2, 1, 3, 2, 4]
```

に書き換えても問題ない。この書き換えを座標圧縮とよぶ。

座標圧縮後の数列の要素の最大値は、数列の要素数以下になる。

```python
def coordinate_compression(L: list) -> list:
    B = sorted(set(L))
    d = {}
    for i in range(len(B)):
        d[B[i]] = i
    compressed_L = []
    for i in range(len(L)):
        compressed_L.append(d[L[i]])
    return compressed_L
```
