---
layout: default
parent: 整数論
summary:
last_modified_date:
---

# 10 進法と n 進法との相互変換

ググって出てきたコードがバグってたのでちゃんと自分で書く。

## 10 進法 → n 進法

- [ABC105C - Base -2 Number](https://atcoder.jp/contests/abc105/tasks/abc105_c)
- [ARC164A - Ternary Decomposition](https://atcoder.jp/contests/arc164/tasks/arc164_a)

$10$ 進数の $x$ を $n$ 進数に変換して、

$$ x = S_0 \times n^0 + S_1 \times n^1 + \cdots + S_k \times n^k $$

を満たすような $S = (S_0, S_1, \dots,S_k)$ を配列として返す。

```python
def int_base(x: int, base: int) -> list:
    assert (base >= 2 or base <= -2)
    if base >= 2:
        assert (x >= 0)
    if x == 0:
        return [0]
    x_base = []
    k = 0
    while True:
        if abs(x) < abs(base) ** k:
            break
        q = (abs(x) % (abs(base)**(k+1))) // (abs(base)**k)
        x_base.append(q)
        x -= q * (base**k)
        k += 1
    return x_base
```

## n 進法 → 10 進法

$2 \leq n \leq 36$ のときは、`int(x, n)` で $x$ を $n$ 進法とみなして $10$ 進法に変換できる。

todo: それ以外のを書く
