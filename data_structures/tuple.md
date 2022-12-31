---
layout: default
parent: データ構造
summary: 
last_modified_date: 2022-12-31
---

# タプル tuple

```python
math = 70
english = 80
japanese = 60
t = (math, english, japanese)  # 順序付きで変更できない
```

|操作|コード|最悪計算量|
|---|:-:|:-:|---|
|$k$ 番目の要素にアクセス| `t[k]` |$O(1)$|
|要素数を取得|`len(t)`|$O(1)$|

- タプルはあくまで「組」を表すためのデータ型なので、要素も順序も変更できない