> Example

Given `f[0..N)` of int, compute an optimal value over all segments `f[i..j)`.

Segment `f[i..j)` means:

```text
i ≤ k < j
```

所以 `j` 是 excluded 的。允许 empty segment，也就是 `i = j`。

两章的共同结构：

- `C.n`：到目前为止，所有 `0 ≤ i ≤ j ≤ n` 的 segments 里的 global best
- `D.n`：所有以 `n` 结尾的 segments 里的 best suffix
- sum 只需要 `D.n`
- product 遇到负数会翻转最大/最小，所以需要同时维护 `D.n` 和 `G.n`

# Minimum Segment Sum

1. Postcondition
   ```text
   r = 〈↓ i, j : 0 ≤ i ≤ j ≤ N : SS.i.j 〉
   ```
   `↓` 表示 minimum。
2. Model the problem domain
   1. 定义 segment sum
      ```text
      * (0) SS.i.j = 〈+ k : i ≤ k < j : f.k 〉, 0 ≤ i ≤ j ≤ N
      ```
   2. 推出 empty segment
      ```text
      SS.i.i
      = {(0)}
      〈+ k : i ≤ k < i : f.k 〉
      = { Empty Range }
      Id+

      (1) SS.i.i = 0, 0 ≤ i ≤ N
      ```
   3. 推出向右扩展一个元素
      ```text
      SS.i.(j+1)
      = {(0)}
      〈+ k : i ≤ k < j+1 : f.k 〉
      = { Split off k = j term }
      〈+ k : i ≤ k < j : f.k 〉 + f.j
      = {(0)}
      SS.i.j + f.j

      (2) SS.i.(j+1) = SS.i.j + f.j, 0 ≤ i ≤ j < N
      ```
   4. 定义 global best
      ```text
      * (3) C.n = 〈↓ i, j : 0 ≤ i ≤ j ≤ n : SS.i.j 〉, 0 ≤ n ≤ N
      ```
   5. 推出C.0
      ```text
      C.0
      = {(3)}
      〈↓ i, j : 0 ≤ i ≤ j ≤ 0 : SS.i.j 〉
      = { 1-point }
      SS.0.0
      = {(1)}
      0

      (4) C.0 = 0
      ```
   6. 推出C.(n+1)
      ```text
      C.(n+1)
      = {(3)}
      〈↓ i, j : 0 ≤ i ≤ j ≤ n+1 : SS.i.j 〉
      = { Split off j = n+1 term }
      〈↓ i, j : 0 ≤ i ≤ j ≤ n : SS.i.j 〉 ↓ 〈↓ i : 0 ≤ i ≤ n+1 : SS.i.(n+1) 〉
      = {(3)}
      C.n ↓ 〈↓ i : 0 ≤ i ≤ n+1 : SS.i.(n+1) 〉
      ```
      这里发现 `C.(n+1)` 需要一个额外的量：所有以 `n+1` 结尾的 segment sum 里的最小值。
   7. 定义 suffix best
      ```text
      * (6) D.n = 〈↓ i : 0 ≤ i ≤ n : SS.i.n 〉, 0 ≤ n ≤ N

      (5) C.(n+1) = C.n ↓ D.(n+1), 0 ≤ n < N
      ```
      `D.n` 表示所有以 `n` 结尾的 segment sum 里的最小值。
   8. 推出D.0
      ```text
      D.0
      = {(6)}
      〈↓ i : 0 ≤ i ≤ 0 : SS.i.0 〉
      = { 1-point }
      SS.0.0
      = {(1)}
      0

      (7) D.0 = 0
      ```
   9. 推出D.(n+1)
      ```text
      D.(n+1)
      = {(6)}
      〈↓ i : 0 ≤ i ≤ n+1 : SS.i.(n+1) 〉
      = { Split off i = n+1 term }
      〈↓ i : 0 ≤ i ≤ n : SS.i.(n+1) 〉 ↓ SS.(n+1).(n+1)
      = {(1)}
      〈↓ i : 0 ≤ i ≤ n : SS.i.(n+1) 〉 ↓ 0
      = {(2)}
      〈↓ i : 0 ≤ i ≤ n : SS.i.n + f.n 〉 ↓ 0
      = { + distributes over ↓ if the range is non-empty }
      (〈↓ i : 0 ≤ i ≤ n : SS.i.n 〉 + f.n) ↓ 0
      = {(6)}
      (D.n + f.n) ↓ 0

      (8) D.(n+1) = (D.n + f.n) ↓ 0, 0 ≤ n < N
      ```
3. Rewrite the postcondition.
   ```text
   Post : r = C.n ∧ n = N
   ```
4. Choose invariants.
   ```text
   P0: r = C.n ∧ d = D.n
   P1: 0 ≤ n ≤ N
   ```
5. Guard
   ```text
   n ≠ N
   ```
6. Establish invariants
   ```text
   n, r, d := 0, 0, 0
   ```
7. Variant
   ```text
   N - n
   ```
8. Loop body
   ```text
   (n, r, d := n+1, E, E').P0
   = { Text Substitution }
   E = C.(n+1) ∧ E' = D.(n+1)
   = {(5)}
   E = C.n ↓ D.(n+1) ∧ E' = D.(n+1)
   = {(8) twice}
   E = C.n ↓ (D.n + f.n) ↓ 0 ∧ E' = (D.n + f.n) ↓ 0
   = { P0 }
   E = r ↓ (d + f.n) ↓ 0 ∧ E' = (d + f.n) ↓ 0
   ```
9. Finished algorithm
   ```text
   n, r, d := 0, 0, 0
   ;do n ≠ N →
       n, r, d := n+1, r ↓ (d + f.n) ↓ 0, (d + f.n) ↓ 0
   od
   ```

# Maximum Segment Product

Precondition:

```text
f[0..N) contains int values
f does not contain 0
```

没有 `0`，所以每个 `f.n` 只有两种情况：

```text
0 < f.n
f.n < 0
```

1. Postcondition
   ```text
   r = 〈↑ i, j : 0 ≤ i ≤ j ≤ N : SP.i.j 〉
   ```
   `↑` 表示 maximum。
2. Model the problem domain
   1. 定义 segment product
      ```text
      * (0) SP.i.j = 〈* k : i ≤ k < j : f.k 〉, 0 ≤ i ≤ j ≤ N
      ```
   2. 推出 empty segment
      ```text
      SP.i.i
      = {(0)}
      〈* k : i ≤ k < i : f.k 〉
      = { Empty Range }
      Id*

      (1) SP.i.i = 1
      ```
   3. 推出向右扩展一个元素
      ```text
      SP.i.(j+1)
      = {(0)}
      〈* k : i ≤ k < j+1 : f.k 〉
      = { Split off k = j term }
      〈* k : i ≤ k < j : f.k 〉 * f.j
      = {(0)}
      SP.i.j * f.j

      (2) SP.i.(j+1) = SP.i.j * f.j, 0 ≤ i ≤ j < N
      ```
   4. 定义 global maximum
      ```text
      * (3) C.n = 〈↑ i, j : 0 ≤ i ≤ j ≤ n : SP.i.j 〉, 0 ≤ n ≤ N
      ```
   5. 推出C.0
      ```text
      C.0
      = {(3)}
      〈↑ i, j : 0 ≤ i ≤ j ≤ 0 : SP.i.j 〉
      = { 1-point }
      SP.0.0
      = {(1)}
      1

      (4) C.0 = 1
      ```
   6. 推出C.(n+1)
      ```text
      C.(n+1)
      = {(3)}
      〈↑ i, j : 0 ≤ i ≤ j ≤ n+1 : SP.i.j 〉
      = { Split off j = n+1 term }
      〈↑ i, j : 0 ≤ i ≤ j ≤ n : SP.i.j 〉 ↑ 〈↑ i : 0 ≤ i ≤ n+1 : SP.i.(n+1) 〉
      = {(3)}
      C.n ↑ 〈↑ i : 0 ≤ i ≤ n+1 : SP.i.(n+1) 〉
      ```
      这里发现 `C.(n+1)` 需要一个额外的量：所有以 `n+1` 结尾的 segment product 里的最大值。
   7. 定义 suffix maximum
      ```text
      * (6) D.n = 〈↑ i : 0 ≤ i ≤ n : SP.i.n 〉, 0 ≤ n ≤ N

      (5) C.(n+1) = C.n ↑ D.(n+1), 0 ≤ n < N
      ```
      `D.n` 是以 `n` 结尾的最大 product。
   8. 推出D.0
      ```text
      D.0
      = {(6)}
      〈↑ i : 0 ≤ i ≤ 0 : SP.i.0 〉
      = { 1-point }
      SP.0.0
      = {(1)}
      1

      (7) D.0 = 1
      ```
   9. 推出D.(n+1)
      先展开定义：
      ```text
      D.(n+1)
      = {(6)}
      〈↑ i : 0 ≤ i ≤ n+1 : SP.i.(n+1) 〉
      = { Split off i = n+1 term }
      〈↑ i : 0 ≤ i ≤ n : SP.i.(n+1) 〉 ↑ SP.(n+1).(n+1)
      = {(1)}
      〈↑ i : 0 ≤ i ≤ n : SP.i.(n+1) 〉 ↑ 1
      = {(2)}
      〈↑ i : 0 ≤ i ≤ n : SP.i.n * f.n 〉 ↑ 1
      ```
      Case `0 < f.n`，乘正数不会翻转大小关系，所以 `* f.n` 可以分配到 `↑` 外面：
      ```text
      D.(n+1)
      = 〈↑ i : 0 ≤ i ≤ n : SP.i.n * f.n 〉 ↑ 1
      = { Case 0 < f.n, */↑ }
      (〈↑ i : 0 ≤ i ≤ n : SP.i.n 〉 * f.n) ↑ 1
      = {(6)}
      (D.n * f.n) ↑ 1

      (8)  D.(n+1) = (D.n * f.n) ↑ 1, 0 ≤ n < N ⇐ 0 < f.n
      ```
      Case `f.n < 0`，乘负数会翻转大小关系，所以 `↑` 会变成 `↓`：
      ```text
      D.(n+1)
      = 〈↑ i : 0 ≤ i ≤ n : SP.i.n * f.n 〉 ↑ 1
      = { Case f.n < 0, */↓ }
      (〈↓ i : 0 ≤ i ≤ n : SP.i.n 〉 * f.n) ↑ 1
      ```
      这里发现只维护 `D.n` 不够，因为需要：
      ```text
      〈↓ i : 0 ≤ i ≤ n : SP.i.n 〉
      ```
   10. 定义 suffix minimum
      ```text
      * (10) G.n = 〈↓ i : 0 ≤ i ≤ n : SP.i.n 〉, 0 ≤ n ≤ N

      D.(n+1)
      = (〈↓ i : 0 ≤ i ≤ n : SP.i.n 〉 * f.n) ↑ 1
      = {(10)}
      (G.n * f.n) ↑ 1

      (9)  D.(n+1) = (G.n * f.n) ↑ 1, 0 ≤ n < N ⇐ f.n < 0
      ```
      `G.n` 是以 `n` 结尾的最小 product。product 要维护 `G.n`，因为负数乘上最小值可能变成最大值。
   11. 推出G.0
      ```text
      G.0
      = {(10)}
      〈↓ i : 0 ≤ i ≤ 0 : SP.i.0 〉
      = { 1-point }
      SP.0.0
      = {(1)}
      1

      (11) G.0 = 1
      ```
   12. 推出G.(n+1)
      ```text
      (12) G.(n+1) = (G.n * f.n) ↓ 1, 0 ≤ n < N ⇐ 0 < f.n
      (13) G.(n+1) = (D.n * f.n) ↓ 1, 0 ≤ n < N ⇐ f.n < 0
      ```
      正数不会翻转大小关系，所以最小来自旧最小 `G.n`；负数会翻转大小关系，所以最小来自旧最大 `D.n`。
3. Rewrite the postcondition.
   ```text
   Post : r = C.n ∧ n = N
   ```
4. Choose invariants.
   ```text
   P0: r = C.n ∧ d = D.n ∧ g = G.n
   P1: 0 ≤ n ≤ N
   ```
5. Guard
   ```text
   n ≠ N
   ```
6. Establish invariants
   ```text
   n, r, d, g := 0, 1, 1, 1
   ```
7. Variant
   ```text
   N - n
   ```
8. Loop body
   1. Case `0 < f.n`
      ```text
      (n, r, d, g := n+1, E, E', E'').P0
      = { Text Substitution }
      E = C.(n+1) ∧ E' = D.(n+1) ∧ E'' = G.(n+1)
      = { Case 0 < f.n, (5), (8), (12) }
      E = C.n ↑ (D.n * f.n) ↑ 1 ∧ E' = (D.n * f.n) ↑ 1 ∧ E'' = (G.n * f.n) ↓ 1
      = { P0 }
      E = r ↑ (d * f.n) ↑ 1 ∧ E' = (d * f.n) ↑ 1 ∧ E'' = (g * f.n) ↓ 1
      ```
   2. Case `f.n < 0`
      ```text
      (n, r, d, g := n+1, E, E', E'').P0
      = { Text Substitution }
      E = C.(n+1) ∧ E' = D.(n+1) ∧ E'' = G.(n+1)
      = { Case f.n < 0, (5), (9), (13) }
      E = C.n ↑ (G.n * f.n) ↑ 1 ∧ E' = (G.n * f.n) ↑ 1 ∧ E'' = (D.n * f.n) ↓ 1
      = { P0 }
      E = r ↑ (g * f.n) ↑ 1 ∧ E' = (g * f.n) ↑ 1 ∧ E'' = (d * f.n) ↓ 1
      ```
9. Finished algorithm
   ```text
   n, r, d, g := 0, 1, 1, 1
   ;do n ≠ N →
       if 0 < f.n →
           n, r, d, g := n+1, r ↑ (d * f.n) ↑ 1, (d * f.n) ↑ 1, (g * f.n) ↓ 1
       [] f.n < 0 →
           n, r, d, g := n+1, r ↑ (g * f.n) ↑ 1, (g * f.n) ↑ 1, (d * f.n) ↓ 1
       fi
   od
   ```

# Quick pattern

Minimum segment sum:

```text
d := best minimum suffix ending at n
r := best minimum segment seen so far

new_d = (d + f.n) ↓ 0
new_r = r ↓ new_d
```

Maximum segment product:

```text
d := best maximum suffix ending at n
g := best minimum suffix ending at n
r := best maximum segment seen so far

if 0 < f.n:
    new_d = (d * f.n) ↑ 1
    new_g = (g * f.n) ↓ 1
    new_r = r ↑ new_d

if f.n < 0:
    new_d = (g * f.n) ↑ 1
    new_g = (d * f.n) ↓ 1
    new_r = r ↑ new_d
```
