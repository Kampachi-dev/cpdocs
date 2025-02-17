---
layout: default
parent: Rust
summary: 
last_modified_date: 2025-02-17
---

# from Python to Rust

いろいろな書き換え

## 入力

[proconio](https://docs.rs/proconio/latest/proconio/index.html) を使う。

```rust
use proconio::input;

input! {
    n: usize,
}
```

### 先頭に0を挟みたいとき

素直に `insert()` する。

```rust
use proconio::input;

input! {
    n: usize,
    p: [usize, n],
}
p.insert(0, 0);
```

```python
N = int(input())
P = [0] + list(map(int, input().split()))
```

## 出力

```rust
// 半角スペース区切り
println!("{}", ans.iter().join(" "));

// 最初の要素だけスキップ
println!("{}", ans.iter().skip(1).join(" "));
```