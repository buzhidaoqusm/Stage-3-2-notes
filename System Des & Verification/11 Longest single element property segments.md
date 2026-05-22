> Example

Given `f[0..N)` of int, determine the length of the longest segment where every single element satisfies some property `Q`.

This corresponds to:

```text
Longest single element property segments
```

The property is about one element at a time:

```text
Q.(f.k)
```

not about adjacent pairs:

```text
PP.(f.(k-1)).(f.k)
```

# Examples

1. Chapter 21: Longest All Even Segment
   ```text
   Q.x ≡ even.x
   ```
2. Chapter 22: Longest All Zero Segment
   ```text
   Q.x ≡ x = 0
   ```
3. Chapter 23: Longest Single Element Property Segments
   ```text
   generic Q.x
   ```

# 通解

1. Precondition
   ```text
   f[0..N) of int contains values
   0 ≤ N
   ```
2. Postcondition
   ```text
   r = 〈↑ i, j : 0 ≤ i ≤ j ≤ N ∧ AQ.i.j : j-i 〉
   ```
3. Model the problem domain
   1. 定义 single element property segment
      ```text
      * (0) AQ.i.j ≡ 〈∀ k : i ≤ k < j : Q.(f.k)〉, 0 ≤ i ≤ j ≤ N
      ```
   2. 推出AQ.i.i
      ```text
      AQ.i.i
      = {(0)}
      〈∀ k : i ≤ k < i : Q.(f.k)〉
      = { Empty Range }
      Id∧

      (1) AQ.i.i, 0 ≤ i ≤ N
      ```
   3. 推出AQ.i.(j+1)
      ```text
      AQ.i.(j+1)
      = {(0)}
      〈∀ k : i ≤ k < j+1 : Q.(f.k)〉
      = { Split off k = j term }
      〈∀ k : i ≤ k < j : Q.(f.k)〉 ∧ Q.(f.j)
      = {(0)}
      AQ.i.j ∧ Q.(f.j)

      (2) AQ.i.(j+1) ≡ AQ.i.j ∧ Q.(f.j), 0 ≤ i ≤ j < N
      ```
   4. 定义 global best
      ```text
      * (3) C.n = 〈↑ i, j : 0 ≤ i ≤ j ≤ n ∧ AQ.i.j : j-i 〉, 0 ≤ n ≤ N
      ```
   5. 推出C.0
      ```text
      C.0
      = {(3)}
      〈↑ i, j : 0 ≤ i ≤ j ≤ 0 ∧ AQ.i.j : j-i 〉
      = { (1) and 1-point }
      0 - 0
      = { arithmetic }
      0

      (4) C.0 = 0
      ```
   6. 推出C.(n+1)
      ```text
      C.(n+1)
      = {(3)}
      〈↑ i, j : 0 ≤ i ≤ j ≤ n+1 ∧ AQ.i.j : j-i 〉
      = { Split off j = n+1 term }
      〈↑ i, j : 0 ≤ i ≤ j ≤ n ∧ AQ.i.j : j-i 〉
        ↑ 〈↑ i : 0 ≤ i ≤ n+1 ∧ AQ.i.(n+1) : (n+1)-i 〉
      = {(3)}
      C.n ↑ 〈↑ i : 0 ≤ i ≤ n+1 ∧ AQ.i.(n+1) : (n+1)-i 〉
      ```
      这里发现需要一个额外的量：所有以 `n` 结尾的 valid segment 的最大长度。
   7. 定义 suffix best
      ```text
      * (6) D.n = 〈↑ i : 0 ≤ i ≤ n ∧ AQ.i.n : n-i 〉, 0 ≤ n ≤ N

      (5) C.(n+1) = C.n ↑ D.(n+1), 0 ≤ n < N
      ```
      `D.n` 表示以 `n` 结尾的最长 valid segment 的长度。
   8. 推出D.0
      ```text
      D.0
      = {(6)}
      〈↑ i : 0 ≤ i ≤ 0 ∧ AQ.i.0 : 0-i 〉
      = { (1) and 1-point }
      0 - 0
      = { arithmetic }
      0

      (7) D.0 = 0
      ```
   9. 推出D.(n+1)
      ```text
      D.(n+1)
      = {(6)}
      〈↑ i : 0 ≤ i ≤ n+1 ∧ AQ.i.(n+1) : (n+1)-i 〉
      = { Split off i = n+1 term }
      〈↑ i : 0 ≤ i ≤ n ∧ AQ.i.(n+1) : (n+1)-i 〉 ↑ 0
      = {(2)}
      〈↑ i : 0 ≤ i ≤ n ∧ AQ.i.n ∧ Q.(f.n) : (n+1)-i 〉 ↑ 0
      ```
      Case `Q.(f.n)`:
      ```text
      D.(n+1)
      = 〈↑ i : 0 ≤ i ≤ n ∧ AQ.i.n : (n+1)-i 〉 ↑ 0
      = { +/↑ }
      (1 + 〈↑ i : 0 ≤ i ≤ n ∧ AQ.i.n : n-i 〉) ↑ 0
      = {(6)}
      (1 + D.n) ↑ 0

      (8) D.(n+1) = (1 + D.n) ↑ 0 ⇐ Q.(f.n), 0 ≤ n < N
      ```
      Case `¬Q.(f.n)`:
      ```text
      D.(n+1)
      = 〈↑ i : false : (n+1)-i 〉 ↑ 0
      = { Empty Range }
      Id↑ ↑ 0
      = { arithmetic }
      0

      (9) D.(n+1) = 0 ⇐ ¬Q.(f.n), 0 ≤ n < N
      ```
4. Rewrite the postcondition using model.
   ```text
   Post : r = C.n ∧ n = N
   ```
5. Choose invariants.
   ```text
   P0: r = C.n ∧ d = D.n
   P1: 0 ≤ n ≤ N
   ```
6. Establish invariants
   ```text
   n, r, d := 0, 0, 0
   ```
7. Guard
   ```text
   n ≠ N
   ```
8. Variant
   ```text
   N - n
   ```
9. Loop body
   1. Case `Q.(f.n)`
      ```text
      (n, r, d := n+1, E, E').P0
      = { Text Substitution }
      E = C.(n+1) ∧ E' = D.(n+1)
      = {(5)}
      E = C.n ↑ D.(n+1) ∧ E' = D.(n+1)
      = { Case Q.(f.n), (8) }
      E = C.n ↑ (1 + D.n) ↑ 0 ∧ E' = (1 + D.n) ↑ 0
      = { P0 }
      E = r ↑ (1 + d) ↑ 0 ∧ E' = (1 + d) ↑ 0
      ```
   2. Case `¬Q.(f.n)`
      ```text
      (n, r, d := n+1, E, E').P0
      = { Text Substitution }
      E = C.(n+1) ∧ E' = D.(n+1)
      = {(5)}
      E = C.n ↑ D.(n+1) ∧ E' = D.(n+1)
      = { Case ¬Q.(f.n), (9) }
      E = C.n ↑ 0 ∧ E' = 0
      = { P0 }
      E = r ↑ 0 ∧ E' = 0
      ```
10. Finished algorithm
    ```text
    n, r, d := 0, 0, 0
    ;do n ≠ N →
        if Q.(f.n) →
            n, r, d := n+1, r ↑ (1+d) ↑ 0, (1+d) ↑ 0
        [] ¬Q.(f.n) →
            n, r, d := n+1, r ↑ 0, 0
        fi
    od
    ```

# Instantiations

Longest all even segment:

```text
Q.x ≡ even.x
```

Algorithm:

```text
n, r, d := 0, 0, 0
;do n ≠ N →
    if even.(f.n) →
        n, r, d := n+1, r ↑ (1+d) ↑ 0, (1+d) ↑ 0
    [] odd.(f.n) →
        n, r, d := n+1, r ↑ 0, 0
    fi
od
```

Longest all zero segment:

```text
Q.x ≡ x = 0
```

Algorithm:

```text
n, r, d := 0, 0, 0
;do n ≠ N →
    if f.n = 0 →
        n, r, d := n+1, r ↑ (1+d) ↑ 0, (1+d) ↑ 0
    [] f.n ≠ 0 →
        n, r, d := n+1, r ↑ 0, 0
    fi
od
```
