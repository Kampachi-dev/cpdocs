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

- > 頂点 $1$ から頂点 $i$ への最短路の長さを $d_i$ とすると $$ d_1 \leq d_2 \leq \dots \leq d_N $$ であることが分かります。よって、各 $i$ に対して頂点 $i + 1$ から頂点 $i$ に長さ $0$ の辺を追加したとしても、頂点 $1$ から各頂点への最短路の長さは変化しません。

  - [第二回全国統一プログラミング王決定戦予選 D - Shortest Path on a Line](https://atcoder.jp/contests/nikkei2019-2-qual/tasks/nikkei2019_2_qual_d)

- 無向グラフのサイクル検出は Union-Find で楽できないか考える

  - [ARC164B - Switching Travel](https://atcoder.jp/contests/arc164/tasks/arc164_b)

- 次々と移動していく系の問題はたくさん移動したあとのことを考える
  - [ABC296E - Transition Game](https://atcoder.jp/contests/abc296/tasks/abc296_e)
  - [ABC311C - Find it!](https://atcoder.jp/contests/abc311/tasks/abc311_c)

## ゲーム問題

- 実は先手/後手の一方が最初に決めた戦略を必ず実行できて、もう一方について考える必要がない
  - [ARC164C - Reversible Card Game](https://atcoder.jp/contests/arc164/tasks/arc164_c)

## 未分類

- 実は該当するものの個数が少ないので、全列挙できる

  - $f(X)=3$ を満たすような $X$ は $2^{63}$ までに $_{63}\mathrm{C}_3$ 個しかない
    - [ARC161B - Exactly Three Bits](https://atcoder.jp/contests/arc161/tasks/arc161_b)
  - 最大ケースでも適切に枝刈りした全探索が間に合う
    - [ABC300D - AABCC](https://atcoder.jp/contests/abc300/tasks/abc300_d)

- 逆側から見る

  - 切るのを逆から考えて、繋げるのを考えると貪欲に決められる
    - [ABC252F - Bread](https://atcoder.jp/contests/abc252/tasks/abc252_f)
  - $S$ の先頭を削除してどこかに挿入して $T$ に一致させるのを逆から考えて、$T$ のどこかを削除して先頭に挿入して $S$ に一致させる
    - [ARC154B - New Place](https://atcoder.jp/contests/arc154/tasks/arc154_b)

- 上位 $m$ 個の総和を保持して比較したいときは優先度付きキューの活用を考える
  - $x$ を優先度付きキューに入れるたびに `res += x` して、サイズが $m$ になるまで最下位を取り除く
    - [ABC312F - Cans and Openers](https://atcoder.jp/contests/abc312/tasks/abc312_f)
