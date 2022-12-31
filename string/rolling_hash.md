---
layout: default
parent: 文字列アルゴリズム
summary: 
last_modified_date: 2022-12-31
---

# ローリングハッシュ

文字列や数列を hash 値に変換することで、文字列同士の比較を $O(1)$ で行う。

恣意的な入力ケースが想定される場合、基数 $B$ は乱数で用意することで攻撃を防ぐことができる。


```python
class RollingHash():
    def __init__(self, S: str, B: int, MOD: int = 2**61-1):
        self._MOD = MOD
        self._base = [pow(B, i, MOD) for i in range(len(S)+1)]
        T = [ord(S[i]) - 96 for i in range(len(S))]
        self._hash = [0]
        for n in range(len(S)):
            self._hash.append((self._hash[n] * B + T[n]) % MOD)

    def hash(self, l: int, r: int) -> int:
        "S の l 文字目から r 文字目までの hash を求める / O(1)"
        return (self._hash[r] - self._hash[l-1] * self._base[r-l+1]) % self._MOD
```