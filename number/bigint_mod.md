---
layout: default
parent: 整数論
summary: 
last_modified_date:
---

# 大きい数のあまりをとる

- [ARC154A - Swap Digit](https://atcoder.jp/contests/arc154/tasks/arc154_a)

桁数が莫大な数を割り算してあまりをとりたいことがある。

```python
# A: 巨大な数を 1 桁ずつ区切って格納した配列
a = A[0]
for n in range(1, N):
    a = (a * 10 + A[n]) % MOD
```