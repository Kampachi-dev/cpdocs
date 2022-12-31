---
layout: default
parent: 文字列アルゴリズム
summary: 
last_modified_date: 2022-12-31
---

# ランレングス圧縮

文字列や数字の並びを圧縮する手法。圧縮・展開のいずれも計算量は $O(|S|)$

- [[競プロ用]ランレングス圧縮まとめ - Qiita](https://qiita.com/Kept1994/items/e9179d1dd7c6455d6883)
- [ABC259-C XX to XXX](https://atcoder.jp/contests/abc259/tasks/abc259_c)


```python
from itertools import groupby

def run_length_encoding_to_list(S: str) -> "list[tuple[str, int]]":
    """
    Compress a string to run-length and return a list containing tuples. / O(|S|)

    example) "aabbbbaaca" -> [('a', 2), ('b', 4), ('a', 2), ('c', 1), ('a', 1)]
    """
    groups = groupby(S)
    encoded_list = []
    for key, value in groups:
        encoded_list.append((key, len(list(value))))
    return encoded_list

def run_length_encoding_to_str(S: str) -> str:
    """
    Compress a string to run-length and return a list containing tuples. / O(|S|)

    example) "aabbbbaaca" -> "a2b4a2c1a1"
    """
    groups = groupby(S)
    encoded_list = []
    for key, value in groups:
        encoded_list.append(key)
        encoded_list.append(str(len(list(value))))
    encoded_str = "".join(encoded_list)
    return encoded_str

def run_length_decoding_from_list(L: "list[tuple[str, int]]") -> str:
    """
    Decompress a run-length compressed list into a string. / O(|S|)

    example) [('a', 2), ('b', 4), ('a', 2), ('c', 1), ('a', 1)] -> "aabbbbaaca"
    """
    decoded_list = []
    for c, n in L:
        decoded_list.append(c * n)
    decoded_str = "".join(decoded_list)
    return decoded_str

def run_length_decoding_from_str(S: str) -> str:
    """
    Decompresses a run-length compressed string and returns it as a string. / O(|S|)

    example) "a2b4a2c1a1" -> "aabbbbaaca"
    """
    decoded_list = []
    index = 0
    c = ""
    num = "0"
    while index != len(S):
        if S[index].isalpha():
            num = int(num)
            decoded_list.append(c * num)
            c = S[index]
            num = ""
        else:
            num += S[index]
        index += 1
    num = int(num)
    decoded_list.append(c * num)
    decoded_str = "".join(decoded_list)
    return decoded_str
```
