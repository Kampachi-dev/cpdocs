---
layout: default
parent: 整数論
summary: 
last_modified_date: 2022-12-31
---

# 素因数分解

試し割り法を用いて素因数分解する。計算量は $O(\sqrt{n})$

- [Pythonで素因数分解（試し割り法）](https://note.nkmk.me/python-prime-factorization/)

```python
def prime_factorize(N: int) -> list:
    "Returns a list of prime factorization results / O(√N)"
    factors = []
    if N == 1:
        return [1]
    for p in range(2, N):
        if p * p > N:
            break
        while N % p == 0:
            factors.append(p)
            N //= p
    if N != 1:
        factors.append(N)
    return factors
```

- 各要素の個数が必要なときは `collections.Counter` を使う

```python
from collections import Counter

c_factors = Counter(prime_factorize(N))
```