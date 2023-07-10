---
layout: default
nav_order:
---

# 問題で使った発想

## (i, j) の組を数え上げるやつ

- $a_i < a_j$ かつ $b_i < b_j$ を満たす $(i, j)$ の組が存在するか判定するときは平面走査
  - [ABC309F - Box in Box](https://atcoder.jp/contests/abc309/tasks/abc309_f)
  - $(a, b)$ の順番がずらせなくてそれぞれの大小関係があれば、座標平面上に点を打って考える

## グラフ

- 無向グラフのサイクル検出は Union-Find で楽できないか考える
  - [ARC164B - Switching Travel](https://atcoder.jp/contests/arc164/tasks/arc164_b)

## ゲーム問題

- 実は先手/後手の一方が最初に決めた戦略を必ず実行できて、もう一方について考える必要がない
  - [ARC164C - Reversible Card Game](https://atcoder.jp/contests/arc164/tasks/arc164_c)
