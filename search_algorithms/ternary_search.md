---
layout: default
parent: 探索アルゴリズム
summary: 
last_modified_date: 2022-12-31
---

# 三分探索

関数 $f$ の極値を探索する。計算量は $O(\log n)$ だが、回数を決め打ちするので実際の計算時間は固定になる。

- [三分探索 - 忘れても大丈夫](https://kyopro.hateblo.jp/entry/2019/04/25/134128)
- [ARC054-B ムーアの法則](https://atcoder.jp/contests/arc054/tasks/arc054_b)


```python
l = -100000000.0
r = 100000000.0

for _ in range(300):  # 回数を決め打ちする
    a = (l*2 + r) / 3
    b = (l + r*2) / 3
    f_a, f_b = f(a), f(b)

    # 上に凸か下に凸かで判定を変える
    if f(a) > f(b):
        r = b
    else:
        l = a

print((l+r)/2)
```