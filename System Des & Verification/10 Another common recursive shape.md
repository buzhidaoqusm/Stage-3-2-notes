> Example

Evaluate `f.N`, where `f` has this recursive shape:

```text
* (0) f.n = c.n                 ⇐ ¬b.n
* (1) f.n = h.n ⊕ f.(g.n)       ⇐ b.n
```

where `⊕` is an associative binary operator with identity `Id⊕`.

这个 shape 的意思：

- `¬b.n` 是 base case：不用继续递归，直接得到 `c.n`
- `b.n` 是 recursive case：先产生一个 contribution `h.n`，然后递归到 `g.n`
- `r` 是 accumulator，用来保存已经收集到的 `h` 项


比如 sum of digits，f.1234 = 4 + f.123：

    开始：f.1234                         = f.N     （r = 0, n = 1234）
    第一轮后：4 + f.123                   = f.N     （r = 4, n = 123）
    第二轮后：4 + 3 + f.12               = f.N     （r = 7, n = 12）
    第三轮后：4 + 3 + 2 + f.1            = f.N     （r = 9, n = 1）
    第四轮后：4 + 3 + 2 + 1 + f.0       = f.N     （r = 10, n = 0）

# 通解

1. Domain model

   ```text
   * (0) f.n = c.n                 ⇐ ¬b.n
   * (1) f.n = h.n ⊕ f.(g.n)       ⇐ b.n
   ```
2. Postcondition

   ```text
   r = f.N
   ```
3. Choose invariants.

   ```text
   P0: r ⊕ f.n = f.N
   ```

   `r` 存已经展开出来的部分	，`f.n` 存还没有展开完的 recursive remainder。
4. Establish invariants

   ```text
   n, r := N, Id⊕
   ```

   因为：
   ```text
   Id⊕ ⊕ f.N = f.N
   ```
5. Guard

   ```text
   b.n
   ```

   当 guard 为假，也就是 `¬b.n`：
   ```text
   P0 ∧ ¬b.n
   = { P0 }
   r ⊕ f.n = f.N ∧ ¬b.n
   = { (0) }
   r ⊕ c.n = f.N
   ```

   所以 loop 结束时可以用 `c.n` 收尾。
6. Loop body

   ```text
   P0 ∧ b.n
   = { (1) }
   r ⊕ (h.n ⊕ f.(g.n)) = f.N
   = { ⊕ associative }
   (r ⊕ h.n) ⊕ f.(g.n) = f.N
   = { WP }
   (n, r := g.n, r ⊕ h.n).P0
   ```
7. Finished algorithm

   ```text
   n, r := N, Id⊕
   ;do b.n →
       n, r := g.n, r ⊕ h.n
   od
   ;r := r ⊕ c.n
   ```

# Termination

这个模板只给出 invariant 和 loop body。每个具体题目还要自己找 variant，证明 loop 会停：

```text
variant decreases when n := g.n
eventually ¬b.n becomes true
```

# Example 1: Sum the digits of an integer

Recursive definition:

```text
* (0) f.n = 0                         ⇐ n = 0
* (1) f.n = (n mod 10) + f.(n div 10) ⇐ n ≠ 0
```

Match the template:

```text
⊕     = +
Id⊕   = 0
b.n   = n ≠ 0
¬b.n  = n = 0
c.n   = 0
h.n   = n mod 10
g.n   = n div 10
```

Algorithm:

```text
n, r := N, 0
;do n ≠ 0 →
    n, r := n div 10, r + (n mod 10)
od
;r := r + 0
```

So:

```text
n, r := N, 0
;do n ≠ 0 →
    n, r := n div 10, r + (n mod 10)
od
```

# Example 2: Count the digits of an integer

Recursive definition:

```text
* (0) f.n = 1                     ⇐ n < 10
* (1) f.n = 1 + f.(n div 10)      ⇐ n > 9
```

Match the template:

```text
⊕     = +
Id⊕   = 0
b.n   = n > 9
¬b.n  = n < 10
c.n   = 1
h.n   = 1
g.n   = n div 10
```

Algorithm:

```text
n, r := N, 0
;do n > 9 →
    n, r := n div 10, r + 1
od
;r := r + 1
```

# Quick pattern

If recursion looks like:

```text
f.n = h.n ⊕ f.(g.n)
```

then iteration usually looks like:

```text
n, r := N, Id⊕
;do recursive-case-condition →
    n, r := g.n, r ⊕ h.n
od
;r := r ⊕ base-case-value
```
