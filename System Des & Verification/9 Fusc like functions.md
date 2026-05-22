> Example

Compute `f.N`, where `f` is a Fusc-like function.

课件第 25 章的 fusc function 是：

```text
fusc.0 = 0
fusc.1 = 1
fusc.(2*n) = fusc.n, 0 < n
fusc.(2*n+1) = fusc.n + fusc.(n+1), 0 < n
```

通解情况：

```text
* (0) f.0 = a
* (1) f.1 = b
* (2) f.(2*n) = c * f.n + d, 0 < n
* (3) f.(2*n+1) = e * f.n + g * f.(n+1) + h, 0 < n
```

标准 fusc 是它的特例：

```text
a = 0
b = 1
c = 1
d = 0
e = 1
g = 1
h = 0
```

注意：`(2)` 和 `(3)` 只能在 `0 < n` 时使用，所以 argument 至少要是 `2`。

# Program shape

和老师课件一样，先把 `N=0` 和 `N=1` 单独处理：

```text
if N = 0 → r := a
[] N = 1 → r := b
[] N > 1 →
    "compute f.N where 1 < N"
fi
```

# 通解

1. Domain model

   ```text
   * (0) f.0 = a
   * (1) f.1 = b
   * (2) f.(2*n) = c * f.n + d, 0 < n
   * (3) f.(2*n+1) = e * f.n + g * f.(n+1) + h, 0 < n
   ```
2. Postcondition

   ```text
   r = f.N
   ```
3. Choose invariants.

   ```text
   P0: α * f.n + β * f.(n+1) + γ = f.N
   P1: 1 ≤ n ≤ N
   ```

   课件里的标准 fusc 没有常数项，所以只需要：

   ```text
   P0: α * fusc.n + β * fusc.(n+1) = fusc.N
   ```

   但通解里面有 `+d` 和 `+h`，所以要多维护 `γ`。
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
   n ≠ 1
   ```

   当 `n = 1`：

   ```text
   P0 ∧ P1 ∧ n = 1
   = {definitions of P0, P1}
   α * f.n + β * f.(n+1) + γ = f.N ∧ 1 <= n <= N ∧ n = 1
   = {leibniz}
   α * f.1 + β * f.2 + γ = f.N ∧ true
   = { (2) }
   α * f.1 + β * (c * f.1 + d) = f.N
   = {defn. fusc}
   α * b + β * (c * b + d) + γ = f.N
   ```

   所以 loop 结束后：

   ```text
   r := α * b + β * (c * b + d) + γ
   ```
6. Variant

   ```text
   n
   ```

   每次把 `n` 变成大约一半，所以 complexity 是 `O(log.N)`。
7. Loop body

   1. Case `even.n`

      ```text
      P0
      = {definition P0}
      α * f.n + β * f.(n+1) + γ = f.N
      = { case analysis, even.n i.e. n = 2*p }
      α * f.(2*p) + β * f.(2*p+1) + γ = f.N
      = { (2), (3) }
      α * (c * f.p + d) + β * (e * f.p + g * f.(p+1) + h) + γ = f.N
      = { */+ }
      α * c * f.p + α * d + β * e * f.p + β * g * f.(p+1) + β * h + γ = f.N
      = { + symmetric, */+ }
      (α * c + β * e) * f.p + (β * g) * f.(p+1) + (α * d + β * h + γ) = f.N
      = { WP }
      (n, α, β, γ := n div 2, α * c + β * e, β * g, α * d + β * h + γ).P0
      ```

      Program fragment:

      ```text
      if even.n →
          n, α, β, γ := n div 2, α * c + β * e, β * g, α * d + β * h + γ
      ```
   2. Case `odd.n`

      ```text
      P0
      =
      α * f.n + β * f.(n+1) + γ = f.N
      = { case analysis, odd.n, n = 2*p+1 }
      α * f.(2*p+1) + β * f.(2*p+2) + γ = f.N
      = { 2*p+2 = 2*(p+1), (2), (3) }
      α * (e * f.p + g * f.(p+1) + h) + β * (c * f.(p+1) + d) + γ = f.N
      = { */+ }
      α * e * f.p + α * g * f.(p+1) + α * h + β * c * f.(p+1) + β * d + γ = f.N
      = { + symmetric, */+ }
      (α * e) * f.p + (α * g + β * c) * f.(p+1) + (α * h + β * d + γ) = f.N
      = { WP }
      (n, α, β, γ := (n-1) div 2, α * e, α * g + β * c, α * h + β * d + γ).P0
      ```

      Program fragment:

      ```text
      if odd.n →
          n, α, β, γ := (n-1) div 2, α * e, α * g + β * c, α * h + β * d + γ
      ```
8. Finished algorithm

   ```text
   if N = 0 → r := a
   [] N = 1 → r := b
   [] N > 1 →
       n, α, β, γ := N, 1, 0, 0
      ;do n ≠ 1 →
           if even.n →
               n, α, β, γ := n div 2, α * c + β * e, β * g, α * d + β * h + γ
           [] odd.n →
               n, α, β, γ := (n-1) div 2, α * e, α * g + β * c, α * h + β * d + γ
           fi
       od
      ;r := α * b + β * (c * b + d) + γ
   fi
   ```

# Standard fusc

For standard fusc:

```text
a = 0, b = 1, c = 1, d = 0, e = 1, g = 1, h = 0
```

通解的 even case 变成：

```text
n, α, β, γ
:= n div 2, α * 1 + β * 1, β * 1, α * 0 + β * 0 + γ
=  n div 2, α + β, β, γ
```

省略一直为 `0` 的 `γ`：

```text
n, α, β := n div 2, α + β, β
```

通解的 odd case 变成：

```text
n, α, β, γ
:= (n-1) div 2, α * 1, α * 1 + β * 1, α * 0 + β * 0 + γ
=  (n-1) div 2, α, α + β, γ
```

省略 `γ`：

```text
n, α, β := (n-1) div 2, α, α + β
```

loop 结束时：

```text
r = α * b + β * (c * b + d) + γ
= α * 1 + β * (1 * 1 + 0) + 0
= α + β
```

所以得到老师课件里的 finished program：

```text
if N = 0 → r := 0
[] N = 1 → r := 1
[] N > 1 →
    n, α, β := N, 1, 0
   ;do n ≠ 1 →
        if even.n → n, α, β := n div 2, α + β, β
        [] odd.n → n, α, β := (n-1) div 2, α, α + β
        fi
    od
   ;r := α + β
fi
```
