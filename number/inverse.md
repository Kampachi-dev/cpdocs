---
layout: default
parent: 整数論
summary: 
last_modified_date: 2023-01-01
---

# mod 逆元

- [競技プログラミングにおける剰余の基礎と mod 逆元](https://www.creativ.xyz/modulo-basic/)
- [ABC275E - Sugoroku 4](https://atcoder.jp/contests/abc275/tasks/abc275_e)
- [ABC280E - Critical Hit](https://atcoder.jp/contests/abc280/tasks/abc280_e)

二項係数の計算、確率 DP や期待値 DP などで、割り算をするかわりに mod 逆元を掛け算する操作が必要になることがある。

> 【フェルマーの小定理】
> $P$ が素数のとき、$P$ の倍数でない整数 $a$ について、$$a^{P-1} \equiv 1 \pmod P$$ が成り立つ。

この式は、$$a \cdot a^{P-2} \equiv 1 \pmod P$$ と変形できるので、$a$ の mod 逆元は $a^{P-2}$ であることがわかる。

python では、組み込み関数 `pow` の第 3 引数に mod を指定できるので、直接的に記述できる。

```python
P = 998244353
inv_a = pow(a, P-2, P)
```
