---
layout: default
nav_order:
last_modified_date: 2023-07-10
---

# 問題で使った発想

## (i, j) の組を数え上げるやつ

- 式変形して変数を分離する

  - [ABC146E - Rem of Sum is Num](https://atcoder.jp/contests/abc146/tasks/abc146_e)
  - [ABC166E - This Message Will Self-Destruct in 5s](https://atcoder.jp/contests/abc166/tasks/abc166_e)

- $a_i < a_j$ かつ $b_i < b_j$ を満たす $(i, j)$ の組が存在するか判定するときは平面走査
  - [ABC309F - Box in Box](https://atcoder.jp/contests/abc309/tasks/abc309_f)
  - $(a, b)$ の順番がずらせなくてそれぞれの大小関係があれば、座標平面上に点を打って考える

## グラフ

- 無向グラフのサイクル検出は Union-Find で楽できないか考える
  - [ARC164B - Switching Travel](https://atcoder.jp/contests/arc164/tasks/arc164_b)

## ゲーム問題

- 実は先手/後手の一方が最初に決めた戦略を必ず実行できて、もう一方について考える必要がない
  - [ARC164C - Reversible Card Game](https://atcoder.jp/contests/arc164/tasks/arc164_c)

## 未分類

- 実は該当するものの個数が少ないので、全列挙できる
  - [ARC161B - Exactly Three Bits](https://atcoder.jp/contests/arc161/tasks/arc161_b)
    - $f(X)=3$ を満たすような $X$ は $2^{63}$ までに $_{63}\mathrm{C}_3$ 個しかない
  - [ABC300D - AABCC](https://atcoder.jp/contests/abc300/tasks/abc300_d)
    - 最大ケースでも適切に枝刈りした全探索が間に合う
