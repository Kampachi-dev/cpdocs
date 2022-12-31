---
layout: default
parent: 探索アルゴリズム
summary: 
last_modified_date: 2022-12-31
---

# 二分探索

いわゆる「めぐる式二分探索」。`ok` と `ng` の境目を決定する。二分探索自体の計算量は $O(\log n)$ で、`is_ok` 関数の計算量を別に考慮する必要がある。

- [二分探索アルゴリズムを一般化 〜 めぐる式二分探索法のススメ 〜 - Qiita](https://qiita.com/drken/items/97e37dd6143e33a64c8c)
- [ABC257-D Jumping Takahashi 2](https://atcoder.jp/contests/abc257/tasks/abc257_d)


```python
def is_ok(mid: int) -> bool:
    # ここで mid が条件を満たすかどうか判定する

# 範囲を考えて十分にカバーできる値をセットする
ng = 0
ok = 4 * 10 ** 9

while abs(ok - ng) > 1:
    mid = (ok + ng) // 2
    if is_ok(mid):
        ok = mid
    else:
        ng = mid

print(ok)
```