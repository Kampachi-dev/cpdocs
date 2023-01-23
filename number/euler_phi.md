---
layout: default
parent: 整数論
summary: 
last_modified_date: 2023-01-23
---

# オイラーの Φ 関数

ある正の整数 $N$ に対して、$N$ 未満で $N$ と互いに素な正の整数の個数を $\phi(N)$ と表す。これをオイラーの $\phi$ 関数という。

たとえば、$12$ 未満で $12$ と互いに素な正の整数は $\lbrace 1,5,7,11 \rbrace$ の $4$ つであるから、$\phi(12) = 4$ である。

```python
def euler_phi(N):
    "N 未満で N と互いに素な正の整数の個数を求める / O(√N)"
    res = N
    x = 2
    while x * x <= N:
        if N % x == 0:
            res = res // x * (x-1)
            while N % x == 0:
                N //= x
        x += 1
    if N > 1:
        res = res // N * (N-1)
    return res
```

## オイラーの定理

整数 $n$ と正の整数 $M$ が互いに素のとき、以下が成り立つ。

$$n^{\phi(M)} \equiv 1 \pmod M$$

これをオイラーの定理という。

## フェルマーの小定理

特に $P$ が素数のとき、$\phi(P) = P-1$ である。したがって、整数 $n$ と素数 $P$ が互いに素のとき、以下が成り立つ。

$$n^{P-1} \equiv 1 \pmod P$$

これをフェルマーの小定理という。