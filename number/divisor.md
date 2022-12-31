---
layout: default
parent: 整数論
summary: 
last_modified_date: 2022-12-31
---

# 約数列挙

約数を列挙する。計算量は $O(\sqrt{n})$

- [約数列挙のアルゴリズム](https://algo-method.com/descriptions/84)
- [ABC249-D Index Trio](https://atcoder.jp/contests/abc249/tasks/abc249_d)

```python
def calc_divisors(N: int) -> set:
    "Enumerate the divisors of N and return their sets. / O(√N)"
    divisors = set()
    for i in range(1, N+1):
        if i*i > N:
            break
        if N % i == 0:
            divisors.add(i)
            divisors.add(N // i)
    return divisors  # set of divisors of N

divisors = calc_divisors(N)
```