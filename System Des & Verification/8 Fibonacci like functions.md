> Example

Compute `f.N`, where `f` is a Fibonacci-like function.

课件第 24 章给的是标准 Fibonacci：

```text
fib.0 = 0
fib.1 = 1
fib.(n+2) = fib.n + fib.(n+1), 0 ≤ n
```

第 27/29 章 exercises 说明老师真正想训练的通用形状是：

```text
f.0 = a
f.1 = b
f.(n+2) = x * f.n + y * f.(n+1) + z, 0 ≤ n
```

老师的 approach：propose an invariant based on the shape of the most complicated part of the definition.

也就是根据 recurrence 里的 `f.n` 和 `f.(n+1)`，选择倒推系数 invariant。

# 通解

1. Domain model

   ```text
   * (0) f.0 = a
   * (1) f.1 = b
   * (2) f.(n+2) = x * f.n + y * f.(n+1) + z, 0 ≤ n
   ```
2. Postcondition

   ```text
   r = f.N
   ```
3. Choose invariants.

   ```text
   P0: α * f.n + β * f.(n+1) + γ = f.N
   P1: 0 ≤ n ≤ N
   ```

   如果 recurrence 里面没有 `+ z`，也就是 `z = 0`，那么 `γ` 可以一直保持 `0`，老师的写法常常省略它：

   ```text
   P0: α * f.n + β * f.(n+1) = f.N
   ```
4. Establish invariants

   ```text
   n, α, β, γ := N, 1, 0, 0
   ```

   因为：

   ```text
   1 * f.N + 0 * f.(N+1) + 0 = f.N
   ```
5. Guard

   ```text
   n ≠ 0
   ```

   我们发现

   ```text
   P0 ∧ P1 ∧ n=0
   = {definitions of P0, P1}
   α * f.n + β * f.(n+1) + γ = f.N ∧ 0 ≤ n ≤ N ∧ n = 0
   = { algebra }
   α * f.0 + β * f.1 + γ = f.N
   = { (0), (1) }
   α * a + β * b + γ = f.N
   ```

   所以 loop 结束后可以用这个简单表达式算出 `f.N`。
6. Variant

   ```text
   n
   ```
7. Loop body
   当 `n ≠ 0` 时，可以把 `f.(n+1)` 往回展开：

   ```text
   f.(n+1) = x * f.(n-1) + y * f.n + z
   ```

   推导：

   ```text
   P0
   =
   α * f.n + β * f.(n+1) + γ = f.N
   = { P1 and guard => 1 ≤ n, so 2 ≤ n+1, (2) }
   α * f.n + β * (x * f.(n-1) + y * f.n + z) + γ = f.N
   = { */+ }
   α * f.n + β * x * f.(n-1) + β * y * f.n + β * z + γ = f.N
   = { algebra, grouping terms }
   β * x * f.(n-1) + (α + β * y) * f.n + (β * z + γ) = f.N
   = { WP }
   (n, α, β, γ := n-1, β * x, α + β * y, β * z + γ).P0
   ```
8. Finished algorithm

   ```text
   n, α, β, γ := N, 1, 0, 0
   ;do n ≠ 0 →
       n, α, β, γ := n-1, β * x, α + β * y, β * z + γ
   od
   ;r := α * a + β * b + γ
   ```

# No constant term

如果 recurrence 是：

```text
f.0 = a
f.1 = b
f.(n+2) = x * f.n + y * f.(n+1)
```

就令 `z = 0`，可以省略 `γ`。

1. Invariants
   ```text
   P0: α * f.n + β * f.(n+1) = f.N
   P1: 0 ≤ n ≤ N
   ```
2. Loop body
   ```text
   α * f.n + β * f.(n+1) = f.N
   = { recurrence }
   α * f.n + β * (x * f.(n-1) + y * f.n) = f.N
   = { */+ }
   β * x * f.(n-1) + (α + β * y) * f.n = f.N
   = { WP }
   (n, α, β := n-1, β * x, α + β * y).P0
   ```
3. Finished algorithm
   ```text
   n, α, β := N, 1, 0
   ;do n ≠ 0 →
       n, α, β := n-1, β * x, α + β * y
   od
   ;r := α * a + β * b
   ```

# Examples from exercises

## Exercise 1

```text
f.0 = 2
f.1 = 7
f.(n+2) = f.n + f.(n+1)
```

So:

```text
a = 2, b = 7, x = 1, y = 1, z = 0
```

Algorithm:

```text
n, α, β := N, 1, 0
;do n ≠ 0 →
    n, α, β := n-1, β, α + β
od
;r := α * 2 + β * 7
```

This matches the teacher's solution:

```text
P0: α * f.n + β * f.(n+1) = f.N
```

## Exercise 2

```text
f.0 = 3
f.1 = 5
f.(n+2) = 2 * f.n + 6 * f.(n+1)
```

So:

```text
a = 3, b = 5, x = 2, y = 6, z = 0
```

Algorithm:

```text
n, α, β := N, 1, 0
;do n ≠ 0 →
    n, α, β := n-1, β * 2, α + β * 6
od
;r := α * 3 + β * 5
```

Teacher-style loop body:

```text
α * f.n + β * f.(n+1) = f.N
= { recurrence }
α * f.n + β * (2 * f.(n-1) + 6 * f.n) = f.N
= { */+ }
β * 2 * f.(n-1) + (α + β * 6) * f.n = f.N
= { WP }
(n, α, β := n-1, β * 2, α + β * 6).P0
```

## Exercise 3

```text
f.0 = 1
f.1 = 4
f.(n+2) = 2 * f.n + 3 * f.(n+1) + 5
```

So:

```text
a = 1, b = 4, x = 2, y = 3, z = 5
```

Because of `+5`, we need `γ`.

Algorithm:

```text
n, α, β, γ := N, 1, 0, 0
;do n ≠ 0 →
    n, α, β, γ := n-1, β * 2, α + β * 3, β * 5 + γ
od
;r := α * 1 + β * 4 + γ
```

Teacher-style loop body:

```text
α * f.n + β * f.(n+1) + γ = f.N
= { recurrence }
α * f.n + β * (2 * f.(n-1) + 3 * f.n + 5) + γ = f.N
= { */+ }
β * 2 * f.(n-1) + (α + β * 3) * f.n + β * 5 + γ = f.N
= { WP }
(n, α, β, γ := n-1, β * 2, α + β * 3, β * 5 + γ).P0
```

# Standard Fibonacci

Standard Fibonacci is:

```text
fib.0 = 0
fib.1 = 1
fib.(n+2) = fib.n + fib.(n+1)
```

So:

```text
a = 0, b = 1, x = 1, y = 1, z = 0
```

Algorithm:

```text
n, α, β := N, 1, 0
;do n ≠ 0 →
    n, α, β := n-1, β, α + β
od
;r := α * 0 + β * 1
```

which simplifies to:

```text
r := β
```

# Note

There is also a forward version that maintains `p = f.n` and `q = f.(n+1)`, but that is not the teacher's method in these slides. For this course, the reusable pattern is the backward coefficient invariant:

```text
P0: α * f.n + β * f.(n+1) + γ = f.N
```
