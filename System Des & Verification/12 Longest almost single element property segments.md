> Example

Given `f[0..N)` of int, determine the length of the longest segment with at most one exceptional element.

This corresponds to:

```text
Longest "almost" single element property segments
```

The property is about one element at a time:

```text
B.(f.k)
```

where `B.x` counts whether element `x` is the exceptional/bad element.

# Example from Chapter 38

Longest almost non-zero segment:

```text
B.x = 1 ⇐ x = 0
B.x = 0 ⇐ x ≠ 0
```

So the segment is allowed to contain at most one zero.

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
   1. 定义 bad-element counter
      ```text
      g.x = 0 ⇐ x satisfies the desired property
      g.x = 1 ⇐ x is an exception
      ```
   2. 定义 almost-valid segment
      ```text
      * (0) AQ.i.j ≡ 〈+ k : i ≤ k < j : g.(f.k)〉 ≤ 1, 0 ≤ i ≤ j ≤ N
      ```
   3. 定义 clean segment
      ```text
      * (1) CQ.i.j ≡ 〈+ k : i ≤ k < j : g.(f.k)〉 = 0, 0 ≤ i ≤ j ≤ N
      ```
      `AQ` 允许最多一个 exception；`CQ` 完全不允许 exception。
   4. 推出AQ.i.i 和 CQ.i.i
      ```text
      AQ.i.i
      = 〈+ k : i ≤ k < i : g.(f.k)〉 ≤ 1
      = { Empty Range }
      0 ≤ 1
      = True

      CQ.i.i
      = 〈+ k : i ≤ k < i : g.(f.k)〉 = 0
      = { Empty Range }
      0 = 0
      = True
      ```
   5. 推出AQ.i.(j+1)
      ```text
      AQ.i.(j+1)
      = 〈+ k : i ≤ k < j+1 : g.(f.k)〉 ≤ 1
      = { Split off k = j term }
      〈+ k : i ≤ k < j : g.(f.k)〉 + g.(f.j) ≤ 1
      ```
      Case `g.(f.j) = 0`:
      ```text
      AQ.i.(j+1)
      ≡ 〈+ k : i ≤ k < j : g.(f.k)〉 + 0 ≤ 1
      ≡ 〈+ k : i ≤ k < j : g.(f.k)〉 ≤ 1
      ≡ AQ.i.j
      ```
      Case `g.(f.j) = 1`:
      ```text
      AQ.i.(j+1)
      ≡ 〈+ k : i ≤ k < j : g.(f.k)〉 + 1 ≤ 1
      ≡ 〈+ k : i ≤ k < j : g.(f.k)〉 = 0
      ≡ CQ.i.j
      ```
   6. 推出CQ.i.(j+1)
      ```text
      CQ.i.(j+1)
      = 〈+ k : i ≤ k < j+1 : g.(f.k)〉 = 0
      = 〈+ k : i ≤ k < j : g.(f.k)〉 + g.(f.j) = 0
      ```
      Case `g.(f.j) = 0`:
      ```text
      CQ.i.(j+1)
      ≡ 〈+ k : i ≤ k < j : g.(f.k)〉 + 0 = 0
      ≡ 〈+ k : i ≤ k < j : g.(f.k)〉 = 0
      ≡ CQ.i.j
      ```
      Case `g.(f.j) = 1`:
      ```text
      CQ.i.(j+1)
      ≡ 〈+ k : i ≤ k < j : g.(f.k)〉 + 1 = 0
      ≡ 〈+ k : i ≤ k < j : g.(f.k)〉 = -1
      = { sum of 0/1 terms is non-negative }
      false
      ```
   7. 定义 global best
      ```text
      * (2) C.n = 〈↑ i, j : 0 ≤ i ≤ j ≤ n ∧ AQ.i.j : j-i 〉, 0 ≤ n ≤ N
      ```
   8. 推出C.0
      ```text
      C.0
      = { (2) }
      〈↑ i, j : 0 ≤ i ≤ j ≤ 0 ∧ AQ.i.j : j-i 〉
      = { 1-point range, i=0, j=0, and AQ.0.0 }
      0-0
      = { arithmetic }
      0
      ```
   9. 推出C.(n+1)
      ```text
      C.(n+1)
      = { (2) }
      〈↑ i, j : 0 ≤ i ≤ j ≤ n+1 ∧ AQ.i.j : j-i 〉
      = { Split off j = n+1 term }
      〈↑ i, j : 0 ≤ i ≤ j ≤ n ∧ AQ.i.j : j-i 〉
        ↑ 〈↑ i : 0 ≤ i ≤ n+1 ∧ AQ.i.(n+1) : (n+1)-i 〉
      = { (2) }
      C.n ↑ 〈↑ i : 0 ≤ i ≤ n+1 ∧ AQ.i.(n+1) : (n+1)-i 〉
      ```
      这里需要一个 almost suffix。
   10. 定义 almost suffix
      ```text
      * (3) D.n = 〈↑ i : 0 ≤ i ≤ n ∧ AQ.i.n : n-i 〉, 0 ≤ n ≤ N

      所以
      C.(n+1)
      = C.n ↑ D.(n+1)
      ```
   11. 定义 clean suffix
      ```text
      * (4) E.n = 〈↑ i : 0 ≤ i ≤ n ∧ CQ.i.n : n-i 〉, 0 ≤ n ≤ N
      ```
      `D.n` 是以 `n` 结尾、最多一个 exception 的最长 suffix。
      
      `E.n` 是以 `n` 结尾、零个 exception 的最长 suffix。
   12. Initial values
      ```text
      D.0
      = { (3) }
      〈↑ i : 0 ≤ i ≤ 0 ∧ AQ.i.0 : 0-i 〉
      = { 1-point range and AQ.0.0 }
      0-0
      = { arithmetic }
      0

      E.0
      = { (4) }
      〈↑ i : 0 ≤ i ≤ 0 ∧ CQ.i.0 : 0-i 〉
      = { 1-point range and CQ.0.0 }
      0-0
      = { arithmetic }
      0

      Therefore:
      C.0 = 0
      D.0 = 0
      E.0 = 0
      ```
   13. 推出D.(n+1) 和 E.(n+1)
      Case current element is good, `g.(f.n) = 0`:
      ```text
      D.(n+1)
      = { (3) }
      〈↑ i : 0 ≤ i ≤ n+1 ∧ AQ.i.(n+1) : (n+1)-i 〉
      = { Split off i = n+1 term }
      〈↑ i : 0 ≤ i ≤ n ∧ AQ.i.(n+1) : (n+1)-i 〉 ↑ ((n+1)-(n+1))
      = { arithmetic }
      〈↑ i : 0 ≤ i ≤ n ∧ AQ.i.(n+1) : (n+1)-i 〉 ↑ 0
      = { g.(f.n)=0, so AQ.i.(n+1) ≡ AQ.i.n }
      〈↑ i : 0 ≤ i ≤ n ∧ AQ.i.n : (n+1)-i 〉 ↑ 0
      = { (n+1)-i = 1+(n-i) }
      (1 + 〈↑ i : 0 ≤ i ≤ n ∧ AQ.i.n : n-i 〉) ↑ 0
      = { (3) }
      (1 + D.n) ↑ 0

      E.(n+1)
      = { (4) }
      〈↑ i : 0 ≤ i ≤ n+1 ∧ CQ.i.(n+1) : (n+1)-i 〉
      = { Split off i = n+1 term }
      〈↑ i : 0 ≤ i ≤ n ∧ CQ.i.(n+1) : (n+1)-i 〉 ↑ 0
      = { g.(f.n)=0, so CQ.i.(n+1) ≡ CQ.i.n }
      〈↑ i : 0 ≤ i ≤ n ∧ CQ.i.n : (n+1)-i 〉 ↑ 0
      = { (n+1)-i = 1+(n-i) }
      (1 + 〈↑ i : 0 ≤ i ≤ n ∧ CQ.i.n : n-i 〉) ↑ 0
      = { (4) }
      (1 + E.n) ↑ 0
      ```
      Case current element is an exception, `g.(f.n) = 1`:
      ```text
      D.(n+1)
      = { (3) }
      〈↑ i : 0 ≤ i ≤ n+1 ∧ AQ.i.(n+1) : (n+1)-i 〉
      = { Split off i = n+1 term }
      〈↑ i : 0 ≤ i ≤ n ∧ AQ.i.(n+1) : (n+1)-i 〉 ↑ 0
      = { g.(f.n)=1, so AQ.i.(n+1) ≡ CQ.i.n }
      〈↑ i : 0 ≤ i ≤ n ∧ CQ.i.n : (n+1)-i 〉 ↑ 0
      = { (n+1)-i = 1+(n-i) }
      (1 + 〈↑ i : 0 ≤ i ≤ n ∧ CQ.i.n : n-i 〉) ↑ 0
      = { (4) }
      (1 + E.n) ↑ 0

      E.(n+1)
      = { (4) }
      〈↑ i : 0 ≤ i ≤ n+1 ∧ CQ.i.(n+1) : (n+1)-i 〉
      = { Split off i = n+1 term }
      〈↑ i : 0 ≤ i ≤ n ∧ CQ.i.(n+1) : (n+1)-i 〉 ↑ 0
      = { g.(f.n)=1, so CQ.i.(n+1) ≡ false }
      〈↑ i : 0 ≤ i ≤ n ∧ false : (n+1)-i 〉 ↑ 0
      = { Empty Range }
      Id↑ ↑ 0
      = { algebra }
      0
      ```
      直觉：遇到一个 exception 时，almost suffix 只能从之前的 clean suffix 接过来。
4. Rewrite the postcondition.
   ```text
   Post : r = C.n ∧ n = N
   ```
5. Choose invariants.
   ```text
   P0: r = C.n ∧ d = D.n ∧ e = E.n
   P1: 0 ≤ n ≤ N
   ```
6. Establish invariants
   ```text
   n, r, d, e := 0, 0, 0, 0
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
   n, r, d, e := n+1, U, U', U''
   ```
   To preserve `P0`, after substitution we need:
   ```text
   U = C.(n+1) ∧ U' = D.(n+1) ∧ U'' = E.(n+1)
   ```
   Case `g.(f.n)=0`:
   ```text
   U = C.(n+1) ∧ U' = D.(n+1) ∧ U'' = E.(n+1)
   = { C.(n+1) = C.n ↑ D.(n+1) }
   U = C.n ↑ D.(n+1) ∧ U' = D.(n+1) ∧ U'' = E.(n+1)
   = { good case for D and E }
   U = C.n ↑ ((1+D.n) ↑ 0) ∧ U' = (1+D.n) ↑ 0 ∧ U'' = (1+E.n) ↑ 0
   = { P0: r=C.n, d=D.n, e=E.n }
   U = r ↑ ((1+d) ↑ 0) ∧ U' = (1+d) ↑ 0 ∧ U'' = (1+e) ↑ 0
   ```
   Case `g.(f.n)=1`:
   ```text
   U = C.(n+1) ∧ U' = D.(n+1) ∧ U'' = E.(n+1)
   = { C.(n+1) = C.n ↑ D.(n+1) }
   U = C.n ↑ D.(n+1) ∧ U' = D.(n+1) ∧ U'' = E.(n+1)
   = { exception case for D and E }
   U = C.n ↑ ((1+E.n) ↑ 0) ∧ U' = (1+E.n) ↑ 0 ∧ U'' = 0
   = { P0: r=C.n, e=E.n }
   U = r ↑ ((1+e) ↑ 0) ∧ U' = (1+e) ↑ 0 ∧ U'' = 0
   ```
10. Finished algorithm
   ```text
   n, r, d, e := 0, 0, 0, 0
   ;do n ≠ N →
       if g.(f.n) = 0 →
           n, r, d, e := n+1, r ↑ (1+d) ↑ 0, (1+d) ↑ 0, (1+e) ↑ 0
       [] g.(f.n) = 1 →
           n, r, d, e := n+1, r ↑ (1+e) ↑ 0, (1+e) ↑ 0, 0
       fi
   od
   ```

# Chapter 38: almost non-zero

```text
g.x = 1 ⇐ x = 0
g.x = 0 ⇐ x ≠ 0
```

Finished algorithm:

```text
n, r, d, e := 0, 0, 0, 0
;do n ≠ N →
    if f.n ≠ 0 →
        n, r, d, e := n+1, r ↑ (1+d) ↑ 0, (1+d) ↑ 0, (1+e) ↑ 0
    [] f.n = 0 →
        n, r, d, e := n+1, r ↑ (1+e) ↑ 0, (1+e) ↑ 0, 0
    fi
od
```
