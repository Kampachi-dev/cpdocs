---
layout: default
parent: 整数論
summary: 
last_modified_date: 2022-12-31
---

# 二項係数

二項係数 $_n \mathrm{C}_r \pmod p$ を求める。前計算で $n!$ と $(n!)^{-1}$ を求めておき、$$_n \mathrm{C}_r = \frac{n!}{r!(n-r)!} = n! \cdot (r!)^{-1} \cdot ((n-r)!)^{-1}$$ と表せることを利用して各クエリを処理する。**$p$ が素数のときのみ使える。**
計算量は前計算 $O(n)$ クエリ $O(1)$

- [Python で二項係数 nCr を高速に計算したい - Satoooh Blog](https://blog.satoooh.com/entry/5195/)
- [ABC178-D Redistribution](https://atcoder.jp/contests/abc178/tasks/abc178_d)


|インスタンス変数|意味|実際の計算|
|:-:|:-:|:-:|
|`fact[n]`|$n! \pmod p$||
|`inv[n]`|$n^{-1} \pmod p$|$(-1) \cdot (p \\% n)^{-1} \cdot (p // n) \pmod p$|
|`factinv[n]`|$(n!)^{-1} \pmod p$||

```python
class BinomialCoefficients():
    def __init__(self, N: int, MOD: int) -> None:
        "Pre-calculate to compute the combination. / O(N)"
        self.MOD = MOD
        self.fact = [1, 1]
        self.inv = [0, 1]
        self.factinv = [1, 1]

        for i in range(2, N+1):
            self.fact.append((self.fact[i-1] * i) % MOD)
            self.inv.append(((-1) * self.inv[MOD % i] * (MOD // i)) % MOD)
            self.factinv.append((self.factinv[i-1] * self.inv[i]) % MOD)

    def calc(self, n: int, r: int) -> int:
        "Calculate the total number of combinations. / O(1)"
        if (r < 0) or (n < r):
            return 0
        return self.fact[n] * self.factinv[r] * self.factinv[n-r] % self.MOD
```

```python
# 必要な分だけ用意する
bc = BinomialCoefficients(2000, 1000000007)
```