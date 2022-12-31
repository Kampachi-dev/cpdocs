---
layout: default
parent: 探索アルゴリズム
summary: 
last_modified_date: 2022-12-31
---

# bit 全探索

$N$ 個の要素のそれぞれについて、選ぶか選ばないかを全探索する。計算量は $O(2^N)$

```python
from itertools import product

for pro in product((0, 1), repeat=N):
    print(pro) # 0, 1 の列
```