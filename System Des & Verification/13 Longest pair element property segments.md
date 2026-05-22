> Example

Given `f[0..N)` of int, determine the length of the longest segment where every adjacent pair satisfies some property `PP`.

This is the shape used by Chapters 32-36:

```text
Longest pair element property segments
```

The property is about adjacent pairs:

```text
PP.(f.(k-1)).(f.k)
```

not about a single element:

```text
Q.(f.k)
```

# 32-37 对应关系

1. Chapter 32: Longest Ascending Segment
   ```text
   PP.x.y ≡ x ≤ y
   ```
2. Chapter 33: Longest Opposite Parity Segment
   ```text
   PP.x.y ≡ (x + y) mod 2 = 1
   ```
3. Chapter 34: Longest Plateau Segment
   ```text
   PP.x.y ≡ x = y
   ```
   Plateau 口头上是“所有值相同”，但课件建模时用的是相邻 pair equality。
4. Chapter 35: Longest Smooth Segment
   ```text
   PP.x.y ≡ |x - y| ≤ 1
   ```
5. Chapter 36: Longest Adjacent Pair Property Segment
   ```text
   generic PP.x.y
   ```
6. Chapter 37: Longest Monotone Segment
   ```text
   MS.i.j ≡ AS.i.j ∨ DS.i.j
   ```
   它是两个 pair-property segment 的组合：
   ```text
   AS: PP.x.y ≡ x ≤ y
   DS: PP.x.y ≡ x ≥ y
   ```

# 通解

1. Precondition
   ```text
   f[0..N) of int contains values
   1 ≤ N
   ```
2. Postcondition
   ```text
   r = 〈↑ i, j : 1 ≤ i ≤ j ≤ N ∧ APP.i.j : j-i 〉 + 1
   ```
   注意 `j-i` 数的是 adjacent pairs 的数量，所以最后要 `+1` 变成 elements 的数量。
3. Model the problem domain
   1. 定义 adjacent pair property segment
      ```text
      * (1) APP.i.j = 〈∀ k : i ≤ k < j : PP.(f.(k-1)).(f.k)〉, 1 ≤ i ≤ j ≤ N
      ```
   2. 推出APP.i.i
      ```text
      APP.i.i
      = {(1)}
      〈∀ k : i ≤ k < i : PP.(f.(k-1)).(f.k)〉
      = { Empty Range }
      Id∧

      (2) APP.i.i, 1 ≤ i ≤ N
      ```
   3. 推出APP.i.(n+1)
      ```text
      APP.i.(n+1)
      = {(1)}
      〈∀ k : i ≤ k < n+1 : PP.(f.(k-1)).(f.k)〉
      = { Split off k = n term }
      〈∀ k : i ≤ k < n : PP.(f.(k-1)).(f.k)〉 ∧ PP.(f.(n-1)).(f.n)
      = {(1)}
      APP.i.n ∧ PP.(f.(n-1)).(f.n)

      (3) APP.i.(n+1) = APP.i.n ∧ PP.(f.(n-1)).(f.n), 1 ≤ i ≤ n < N
      ```
   4. 定义 global best
      ```text
      * (4) C.n = 〈↑ i, j : 1 ≤ i ≤ j ≤ n ∧ APP.i.j : j-i 〉, 1 ≤ n ≤ N
      ```
   5. 推出C.1
      ```text
      C.1
      = {(4)}
      〈↑ i, j : 1 ≤ i ≤ j ≤ 1 ∧ APP.i.j : j-i 〉
      = { (2) and 1-point }
      1 - 1
      = { arithmetic }
      0

      (5) C.1 = 0
      ```
   6. 推出C.(n+1)
      ```text
      C.(n+1)
      = {(4)}
      〈↑ i, j : 1 ≤ i ≤ j ≤ n+1 ∧ APP.i.j : j-i 〉
      = { Split off j = n+1 term }
      〈↑ i, j : 1 ≤ i ≤ j ≤ n ∧ APP.i.j : j-i 〉
        ↑ 〈↑ i : 1 ≤ i ≤ n+1 ∧ APP.i.(n+1) : (n+1)-i 〉
      = {(4)}
      C.n ↑ 〈↑ i : 1 ≤ i ≤ n+1 ∧ APP.i.(n+1) : (n+1)-i 〉
      ```
      这里发现需要一个额外的量：所有以 `n` 结尾的 valid pair segment 的最大 pair-length。
   7. 定义 suffix best
      ```text
      * (6) D.n = 〈↑ i : 1 ≤ i ≤ n ∧ APP.i.n : n-i 〉, 1 ≤ n ≤ N

      (7) C.(n+1) = C.n ↑ D.(n+1), 1 ≤ n < N
      ```
      `D.n` 表示以 `n` 结尾的最长 valid adjacent-pair segment 的 pair-length。
   8. 推出D.1
      ```text
      D.1
      = {(6)}
      〈↑ i : 1 ≤ i ≤ 1 ∧ APP.i.1 : 1-i 〉
      = { (2) and 1-point }
      1 - 1
      = { arithmetic }
      0

      (8) D.1 = 0
      ```
   9. 推出D.(n+1)
      ```text
      D.(n+1)
      = {(6)}
      〈↑ i : 1 ≤ i ≤ n+1 ∧ APP.i.(n+1) : (n+1)-i 〉
      = { Split off i = n+1 term }
      〈↑ i : 1 ≤ i ≤ n ∧ APP.i.(n+1) : (n+1)-i 〉 ↑ 0
      = {(3)}
      〈↑ i : 1 ≤ i ≤ n ∧ APP.i.n ∧ PP.(f.(n-1)).(f.n) : (n+1)-i 〉 ↑ 0
      ```
      Case `PP.(f.(n-1)).(f.n)`:
      ```text
      D.(n+1)
      = 〈↑ i : 1 ≤ i ≤ n ∧ APP.i.n : (n+1)-i 〉 ↑ 0
      = { +/↑ }
      (1 + 〈↑ i : 1 ≤ i ≤ n ∧ APP.i.n : n-i 〉) ↑ 0
      = {(6)}
      (1 + D.n) ↑ 0

      (9) D.(n+1) = (1 + D.n) ↑ 0 ⇐ PP.(f.(n-1)).(f.n)
      ```
      Case `¬PP.(f.(n-1)).(f.n)`:
      ```text
      D.(n+1)
      = 〈↑ i : false : (n+1)-i 〉 ↑ 0
      = { Empty Range }
      Id↑ ↑ 0
      = { arithmetic }
      0

      (10) D.(n+1) = 0 ⇐ ¬PP.(f.(n-1)).(f.n)
      ```
4. Rewrite the postcondition using model.
   ```text
   Post : r = C.n + 1 ∧ n = N
   ```
5. Choose invariants.
   ```text
   P0: r = C.n ∧ d = D.n
   P1: 1 ≤ n ≤ N
   ```
6. Guard
   ```text
   n ≠ N
   ```
7. Establish invariants
   ```text
   n, r, d := 1, 0, 0
   ```
8. Variant
   ```text
   N - n
   ```
9. Loop body
   1. Case `PP.(f.(n-1)).(f.n)`
      ```text
      (n, r, d := n+1, E, E').P0
      = { Text Substitution }
      E = C.(n+1) ∧ E' = D.(n+1)
      = {(7)}
      E = C.n ↑ D.(n+1) ∧ E' = D.(n+1)
      = { Case PP.(f.(n-1)).(f.n), (9) }
      E = C.n ↑ (1 + D.n) ↑ 0 ∧ E' = (1 + D.n) ↑ 0
      = { P0 }
      E = r ↑ (1 + d) ↑ 0 ∧ E' = (1 + d) ↑ 0
      ```
   2. Case `¬PP.(f.(n-1)).(f.n)`
      ```text
      (n, r, d := n+1, E, E').P0
      = { Text Substitution }
      E = C.(n+1) ∧ E' = D.(n+1)
      = {(7)}
      E = C.n ↑ D.(n+1) ∧ E' = D.(n+1)
      = { Case ¬PP.(f.(n-1)).(f.n), (10) }
      E = C.n ↑ 0 ∧ E' = 0
      = { P0 }
      E = r ↑ 0 ∧ E' = 0
      ```
10. Finished algorithm
    ```text
    n, r, d := 1, 0, 0
    ;do n ≠ N →
        if PP.(f.(n-1)).(f.n) →
            n, r, d := n+1, r ↑ (1+d) ↑ 0, (1+d) ↑ 0
        [] ¬PP.(f.(n-1)).(f.n) →
            n, r, d := n+1, r ↑ 0, 0
        fi
    od
    ;r := r + 1
    ```

# Instantiations

```text
Ascending:
PP.x.y ≡ x ≤ y

Opposite parity:
PP.x.y ≡ (x + y) mod 2 = 1

Plateau:
PP.x.y ≡ x = y

Smooth:
PP.x.y ≡ |x - y| ≤ 1
```

# Chapter 37: Monotone segment

Monotone segment is not just one `PP`; it is:

```text
MS.i.j = AS.i.j ∨ DS.i.j
```

where:

```text
AS.i.j = 〈∀ k : i ≤ k < j : f.(k-1) ≤ f.k〉
DS.i.j = 〈∀ k : i ≤ k < j : f.(k-1) ≥ f.k〉
```

So the model needs two suffix variables:

```text
d = D.n   -- longest ascending suffix pair-length ending at n
e = E.n   -- longest descending suffix pair-length ending at n
```

Finished algorithm:

```text
n, r, d, e := 1, 0, 0, 0
;do n ≠ N →
    if f.(n-1) < f.n →
        n, r, d, e := n+1, r ↑ (1+d), (1+d), 0
    [] f.(n-1) = f.n →
        n, r, d, e := n+1, r ↑ (1+d) ↑ (1+e), (1+d), (1+e)
    [] f.(n-1) > f.n →
        n, r, d, e := n+1, r ↑ (1+e), 0, (1+e)
    fi
od
;r := r + 1
```
