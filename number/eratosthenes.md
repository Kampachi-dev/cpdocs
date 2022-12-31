---
layout: default
parent: 整数論
summary: 
last_modified_date: 2022-12-31
---

# 素数列挙

エラトステネスの篩を用いて素数を列挙する。計算量は $O(N \log \log N)$

- [エラトステネスの篩 (ふるい) を徹底解説 〜 実装から計算量まで 〜](https://algo-method.com/descriptions/64)
- [ABC250-D 250-like Number](https://atcoder.jp/contests/abc250/tasks/abc250_d)


```python
def eratosthenes(N: int) -> list:
    "Enumerate prime numbers using the sieve of Eratosthenes. / O(N log log N)"
    is_prime = [True] * (N+1)
    is_prime[0] = False
    is_prime[1] = False
    for n in range(2, N+1):
        if not is_prime[n]:
            continue
        m = n + n
        while m <= N:
            is_prime[m] = False
            m += n
    return is_prime  # is_prime[index]: True if index is prime

is_prime = eratosthenes(N)
```


## 区間篩

制約がきつい場合は、エラトステネスの区間篩を用いて閉区間 $[A, B]$ の素数を列挙する。計算量は $O(\sqrt{B}+B-A) \log \log B)$

- [エラトステネスの区間篩 | アルゴ式](https://algo-method.com/tasks/332/editorial)


```python
def eratosthenes_interval(A: int, B: int) -> list:
    "Enumerate prime numbers in the closed interval [A, B] the sieve of Eratosthenes. / O(√B + B-A) log log B)"
    sqrt_B = ceil(sqrt(B))
    is_prime = eratosthenes(sqrt_B)

    # is_prime_AB[v-A]: True if v in the closed interval [A, B] is prime
    is_prime_AB = [True] * (B-A+1)
    for n in range(2, sqrt_B+1):
        if not is_prime[n]:
            continue
        m = n * ((A+n-1) // n)
        if m == n:
            m += n
        while m <= B:
            is_prime_AB[m-A] = False
            m += n
    return is_prime_AB
```
