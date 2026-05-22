> Example

Given `f[0..N)` of int, determine the length of the longest segment with at most one bad adjacent pair.

This corresponds to:

```text
Longest "almost" pair element property segments
```

The property is about adjacent pairs:

```text
PP.(f.(k-1)).(f.k)
```

# Example from Chapter 39

Longest almost ascending segment:

```text
PP.x.y ≡ x ≤ y
```

A bad pair is an inversion:

```text
x > y
```

The segment is allowed to contain at most one inversion.

# 通解

1. Precondition
   ```text
   f[0..N) of int contains values
   1 ≤ N
   ```
2. Postcondition
   ```text
   r = 〈↑ i, j : 1 ≤ i ≤ j ≤ N ∧ AAPP.i.j : j-i 〉 + 1
   ```
   `j-i` counts adjacent pairs, so the final answer is `+1` elements.
3. Model the problem domain
   1. 定义 bad-pair counter
      ```text
      g.x.y = 0 ⇐ PP.x.y
      g.x.y = 1 ⇐ ¬PP.x.y
      ```
   2. 定义 almost-valid pair segment
      ```text
      * (0) AAPP.i.j ≡ 〈+ k : i ≤ k < j : g.(f.(k-1)).(f.k)〉 ≤ 1, 1 ≤ i ≤ j ≤ N
      ```
   3. 定义 clean pair segment
      ```text
      * (1) APP.i.j ≡ 〈+ k : i ≤ k < j : g.(f.(k-1)).(f.k)〉 = 0, 1 ≤ i ≤ j ≤ N
      ```
      `AAPP` 允许最多一个 bad pair；`APP` 完全不允许 bad pair。
   4. 推出AAPP.i.i 和 APP.i.i
      ```text
      AAPP.i.i
      = 〈+ k : i ≤ k < i : g.(f.(k-1)).(f.k)〉 ≤ 1
      = { Empty Range }
      0 ≤ 1
      = True

      APP.i.i
      = 〈+ k : i ≤ k < i : g.(f.(k-1)).(f.k)〉 = 0
      = { Empty Range }
      0 = 0
      = True
      ```
   5. 推出AAPP.i.(n+1)
      ```text
      AAPP.i.(n+1)
      = 〈+ k : i ≤ k < n+1 : g.(f.(k-1)).(f.k)〉 ≤ 1
      = { Split off k = n term }
      〈+ k : i ≤ k < n : g.(f.(k-1)).(f.k)〉 + g.(f.(n-1)).(f.n) ≤ 1
      ```
      Case `PP.(f.(n-1)).(f.n)`:
      ```text
      AAPP.i.(n+1)
      ≡ 〈+ k : i ≤ k < n : g.(f.(k-1)).(f.k)〉 + 0 ≤ 1
      ≡ 〈+ k : i ≤ k < n : g.(f.(k-1)).(f.k)〉 ≤ 1
      ≡ AAPP.i.n
      ```
      Case `¬PP.(f.(n-1)).(f.n)`:
      ```text
      AAPP.i.(n+1)
      ≡ 〈+ k : i ≤ k < n : g.(f.(k-1)).(f.k)〉 + 1 ≤ 1
      ≡ 〈+ k : i ≤ k < n : g.(f.(k-1)).(f.k)〉 = 0
      ≡ APP.i.n
      ```
   6. 推出APP.i.(n+1)
      ```text
      APP.i.(n+1)
      = 〈+ k : i ≤ k < n+1 : g.(f.(k-1)).(f.k)〉 = 0
      = { Split off k = n term }
      〈+ k : i ≤ k < n : g.(f.(k-1)).(f.k)〉 + g.(f.(n-1)).(f.n) = 0
      ```
      Case `PP.(f.(n-1)).(f.n)`:
      ```text
      APP.i.(n+1)
      ≡ 〈+ k : i ≤ k < n : g.(f.(k-1)).(f.k)〉 + 0 = 0
      ≡ 〈+ k : i ≤ k < n : g.(f.(k-1)).(f.k)〉 = 0
      ≡ APP.i.n
      ```
      Case `¬PP.(f.(n-1)).(f.n)`:
      ```text
      APP.i.(n+1)
      ≡ 〈+ k : i ≤ k < n : g.(f.(k-1)).(f.k)〉 + 1 = 0
      ≡ 〈+ k : i ≤ k < n : g.(f.(k-1)).(f.k)〉 = -1
      = { sum of 0/1 terms is non-negative }
      false
      ```
   7. 定义 global best
      ```text
      * (2) C.n = 〈↑ i, j : 1 ≤ i ≤ j ≤ n ∧ AAPP.i.j : j-i 〉, 1 ≤ n ≤ N
      ```
   8. 推出C.1
      ```text
      C.1
      = { (2) }
      〈↑ i, j : 1 ≤ i ≤ j ≤ 1 ∧ AAPP.i.j : j-i 〉
      = { 1-point range, i=1, j=1, and AAPP.1.1 }
      1-1
      = { arithmetic }
      0
      ```
   9. 推出C.(n+1)
      ```text
      C.(n+1)
      = { (2) }
      〈↑ i, j : 1 ≤ i ≤ j ≤ n+1 ∧ AAPP.i.j : j-i 〉
      = { Split off j = n+1 term }
      〈↑ i, j : 1 ≤ i ≤ j ≤ n ∧ AAPP.i.j : j-i 〉
        ↑ 〈↑ i : 1 ≤ i ≤ n+1 ∧ AAPP.i.(n+1) : (n+1)-i 〉
      = { (2) }
      C.n ↑ 〈↑ i : 1 ≤ i ≤ n+1 ∧ AAPP.i.(n+1) : (n+1)-i 〉
      ```
      这里需要一个 almost suffix。
   10. 定义 almost suffix
      ```text
      * (3) D.n = 〈↑ i : 1 ≤ i ≤ n ∧ AAPP.i.n : n-i 〉, 1 ≤ n ≤ N

      所以
      C.(n+1)
      = C.n ↑ D.(n+1)
      ```
   11. 定义 clean suffix
      ```text
      * (4) G.n = 〈↑ i : 1 ≤ i ≤ n ∧ APP.i.n : n-i 〉, 1 ≤ n ≤ N
      ```
      `D.n` 是以 `n` 结尾、最多一个 bad pair 的最长 suffix pair-length。
      
      `G.n` 是以 `n` 结尾、零个 bad pair 的最长 suffix pair-length。
   12. Initial values
      ```text
      D.1
      = { (3) }
      〈↑ i : 1 ≤ i ≤ 1 ∧ AAPP.i.1 : 1-i 〉
      = { 1-point range and AAPP.1.1 }
      1-1
      = { arithmetic }
      0

      G.1
      = { (4) }
      〈↑ i : 1 ≤ i ≤ 1 ∧ APP.i.1 : 1-i 〉
      = { 1-point range and APP.1.1 }
      1-1
      = { arithmetic }
      0

      Therefore:
      C.1 = 0
      D.1 = 0
      G.1 = 0
      ```
   13. 推出D.(n+1) 和 G.(n+1)
      Case current pair is good, `PP.(f.(n-1)).(f.n)`:
      ```text
      D.(n+1)
      = { (3) }
      〈↑ i : 1 ≤ i ≤ n+1 ∧ AAPP.i.(n+1) : (n+1)-i 〉
      = { Split off i = n+1 term }
      〈↑ i : 1 ≤ i ≤ n ∧ AAPP.i.(n+1) : (n+1)-i 〉 ↑ ((n+1)-(n+1))
      = { arithmetic }
      〈↑ i : 1 ≤ i ≤ n ∧ AAPP.i.(n+1) : (n+1)-i 〉 ↑ 0
      = { good pair, so AAPP.i.(n+1) ≡ AAPP.i.n }
      〈↑ i : 1 ≤ i ≤ n ∧ AAPP.i.n : (n+1)-i 〉 ↑ 0
      = { (n+1)-i = 1+(n-i) }
      (1 + 〈↑ i : 1 ≤ i ≤ n ∧ AAPP.i.n : n-i 〉) ↑ 0
      = { (3) }
      (1 + D.n) ↑ 0

      G.(n+1)
      = { (4) }
      〈↑ i : 1 ≤ i ≤ n+1 ∧ APP.i.(n+1) : (n+1)-i 〉
      = { Split off i = n+1 term }
      〈↑ i : 1 ≤ i ≤ n ∧ APP.i.(n+1) : (n+1)-i 〉 ↑ 0
      = { good pair, so APP.i.(n+1) ≡ APP.i.n }
      〈↑ i : 1 ≤ i ≤ n ∧ APP.i.n : (n+1)-i 〉 ↑ 0
      = { (n+1)-i = 1+(n-i) }
      (1 + 〈↑ i : 1 ≤ i ≤ n ∧ APP.i.n : n-i 〉) ↑ 0
      = { (4) }
      (1 + G.n) ↑ 0
      ```
      Case current pair is bad, `¬PP.(f.(n-1)).(f.n)`:
      ```text
      D.(n+1)
      = { (3) }
      〈↑ i : 1 ≤ i ≤ n+1 ∧ AAPP.i.(n+1) : (n+1)-i 〉
      = { Split off i = n+1 term }
      〈↑ i : 1 ≤ i ≤ n ∧ AAPP.i.(n+1) : (n+1)-i 〉 ↑ 0
      = { bad pair, so AAPP.i.(n+1) ≡ APP.i.n }
      〈↑ i : 1 ≤ i ≤ n ∧ APP.i.n : (n+1)-i 〉 ↑ 0
      = { (n+1)-i = 1+(n-i) }
      (1 + 〈↑ i : 1 ≤ i ≤ n ∧ APP.i.n : n-i 〉) ↑ 0
      = { (4) }
      (1 + G.n) ↑ 0

      G.(n+1)
      = { (4) }
      〈↑ i : 1 ≤ i ≤ n+1 ∧ APP.i.(n+1) : (n+1)-i 〉
      = { Split off i = n+1 term }
      〈↑ i : 1 ≤ i ≤ n ∧ APP.i.(n+1) : (n+1)-i 〉 ↑ 0
      = { bad pair, so APP.i.(n+1) ≡ false }
      〈↑ i : 1 ≤ i ≤ n ∧ false : (n+1)-i 〉 ↑ 0
      = { Empty Range }
      Id↑ ↑ 0
      = { algebra }
      0
      ```
      直觉：遇到一个 bad pair 时，almost suffix 只能从之前的 clean suffix 接过来。
4. Rewrite the postcondition.
   ```text
   Post : r = C.n + 1 ∧ n = N
   ```
5. Choose invariants.
   ```text
   P0: r = C.n ∧ d = D.n ∧ g = G.n
   P1: 1 ≤ n ≤ N
   ```
6. Establish invariants
   ```text
   n, r, d, g := 1, 0, 0, 0
   ```
7. Guard
   ```text
   n ≠ N
   ```
8. Variant
   ```text
   N - n
   ```
9. Loop body derivation
   Let the assignment be:
   ```text
   n, r, d, g := n+1, U, U', U''
   ```
   To preserve `P0`, after substitution we need:
   ```text
   U = C.(n+1) ∧ U' = D.(n+1) ∧ U'' = G.(n+1)
   ```
   Case `PP.(f.(n-1)).(f.n)`:
   ```text
   U = C.(n+1) ∧ U' = D.(n+1) ∧ U'' = G.(n+1)
   = { C.(n+1) = C.n ↑ D.(n+1) }
   U = C.n ↑ D.(n+1) ∧ U' = D.(n+1) ∧ U'' = G.(n+1)
   = { good pair case for D and G }
   U = C.n ↑ ((1+D.n) ↑ 0) ∧ U' = (1+D.n) ↑ 0 ∧ U'' = (1+G.n) ↑ 0
   = { P0: r=C.n, d=D.n, g=G.n }
   U = r ↑ ((1+d) ↑ 0) ∧ U' = (1+d) ↑ 0 ∧ U'' = (1+g) ↑ 0
   ```
   Case `¬PP.(f.(n-1)).(f.n)`:
   ```text
   U = C.(n+1) ∧ U' = D.(n+1) ∧ U'' = G.(n+1)
   = { C.(n+1) = C.n ↑ D.(n+1) }
   U = C.n ↑ D.(n+1) ∧ U' = D.(n+1) ∧ U'' = G.(n+1)
   = { bad pair case for D and G }
   U = C.n ↑ ((1+G.n) ↑ 0) ∧ U' = (1+G.n) ↑ 0 ∧ U'' = 0
   = { P0: r=C.n, g=G.n }
   U = r ↑ ((1+g) ↑ 0) ∧ U' = (1+g) ↑ 0 ∧ U'' = 0
   ```
10. Finished algorithm
   ```text
   n, r, d, g := 1, 0, 0, 0
   ;do n ≠ N →
       if PP.(f.(n-1)).(f.n) →
           n, r, d, g := n+1, r ↑ (1+d) ↑ 0, (1+d) ↑ 0, (1+g) ↑ 0
       [] ¬PP.(f.(n-1)).(f.n) →
           n, r, d, g := n+1, r ↑ (1+g) ↑ 0, (1+g) ↑ 0, 0
       fi
   od
   ;r := r + 1
   ```

# Chapter 39: almost ascending

```text
PP.x.y ≡ x ≤ y
¬PP.x.y ≡ x > y
```

Finished algorithm:

```text
n, r, d, g := 1, 0, 0, 0
;do n ≠ N →
    if f.(n-1) ≤ f.n →
        n, r, d, g := n+1, r ↑ (1+d) ↑ 0, (1+d) ↑ 0, (1+g) ↑ 0
    [] f.(n-1) > f.n →
        n, r, d, g := n+1, r ↑ (1+g) ↑ 0, (1+g) ↑ 0, 0
    fi
od
;r := r + 1
```
