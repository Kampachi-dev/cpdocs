---
layout: default
parent: 動的計画法
summary: 
last_modified_date: 2023-01-01
---

# ヒストグラム中の最大長方形

幅 $N$ のヒストグラムに含まれる長方形の面積の最大値を $O(N)$ で求める。

- [ALGORITHM NOTE ヒストグラム中の最大の長方形の面積](http://algorithms.blog55.fc2.com/blog-entry-132.html)
- [ABC189-C Mandarin Orange](https://atcoder.jp/contests/abc189/tasks/abc189_c)
    - 想定解は $O(N^2)$

まだできあがっていない長方形の左辺をスタックに積んでいく。ヒストグラムの各長方形 `rect` の位置を `pos` とする。

```
if スタックが空:
    スタックに rect を追加
else:
    if (スタックの頂点の長方形の高さ) < (rect の高さ):
        スタックに rect を追加
        pos はそのまま
    if (スタックの頂点の長方形の高さ) == (rect の高さ):
        なにもしない
    if (スタックの頂点の長方形の高さ) > (rect の高さ):
        (スタックの頂点の長方形の高さ) < (rect の高さ) になるまで、スタックから長方形を取り出す
        面積を計算して最大値を更新
        スタックに rect を追加
        pos は最後に取り出した長方形の pos の値とする
```

```python
# N: 要素数
# A[n]: n 番目のヒストグラムの高さ

stack = []  # [(height, pos), ...]
ans = 0
for i in range(N):
    if len(stack) == 0:
        stack.append((A[i], i))
    if A[i] > stack[-1][0]:
        stack.append((A[i], i))
    elif A[i] < stack[-1][0]:
        pos = 0
        while stack and A[i] <= stack[-1][0]:
            area = stack[-1][0] * (i - stack[-1][1])
            ans = max(ans, area)
            pos = stack[-1][1]
            stack.pop()
        stack.append((A[i], pos))

while stack:
    area = stack[-1][0] * (N - stack[-1][1])
    ans = max(ans, area)
    stack.pop()

print(ans)
```