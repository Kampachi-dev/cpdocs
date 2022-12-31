---
layout: default
parent: 探索アルゴリズム
summary: 
last_modified_date: 2022-12-31
---

# 尺取り法

配列の半開区間 $[l, r)$ を探索する。$l$ と $r$ をそれぞれずらしていくことで、探索回数を $O(n)$ に抑える。

```python
l, r = 0, 0

while l < N:
    if r == N or 条件を満たさない:
        # なんか処理
        l += 1
    else:
        # なんか処理
        r += 1
```