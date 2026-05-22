> Example

Given an array `f[0..N)` of int and a target value `X`.
Find the smallest index `i` such that `f.i = X`, if such an index exists.

和 [[4 Linear Search]] 的区别：这里没有保证 `X` 一定存在。

Precondition:

```text
f[0..N) has values
1 ≤ N
```

在这个例子里，`Q` 就是“元素等于目标值 `X`”：

```text
Q.x ≡ x = X
Q.(f.i) ≡ f.i = X
```

如果 `X` 存在：

```text
〈∀ j : 0 ≤ j < i : f.j ≠ X 〉 ∧ f.i = X
```

如果 `X` 不存在，搜索会停在最后一个位置：

```text
〈∀ j : 0 ≤ j < i : f.j ≠ X 〉 ∧ f.i ≠ X ∧ i = N-1
```

把两个情况合起来：

```text
〈∀ j : 0 ≤ j < i : f.j ≠ X 〉 ∧ (f.i = X ∨ (f.i ≠ X ∧ i = N-1))
```

用 law `X ∨ (¬X ∧ Y) ≡ X ∨ Y` 化简：

```text
Post : 〈∀ j : 0 ≤ j < i : f.j ≠ X 〉 ∧ (f.i = X ∨ i = N-1)
```

统一格式：在有限、有序、非空的范围 `f[α..β)` 中，找到第一个满足 predicate `Q` 的位置；如果不存在，就停在最后一个位置。

`Q` 是定义在元素上的 predicate，用来描述我们要找的性质：

```text
Q.x ≡ element x satisfies the property we are searching for
Q.(f.i) ≡ f.i satisfies Q
```

```text
Pre : α < β
Post: 〈∀ j : α ≤ j < i : ¬Q.(f.j) 〉 ∧ (Q.(f.i) ∨ i = β-1)
```

为什么 bounded 版本的 postcondition 后面是 `(Q.(f.i) ∨ i = β-1)`：

- 如果找到了，`Q.(f.i)` 为真
- 如果没找到，guard 不能继续往后走，所以停在最后一个位置 `β-1`
- 前面的 `〈∀ j : α ≤ j < i : ¬Q.(f.j) 〉` 说明 `i` 前面都不满足 `Q`，所以找到时一定是第一个

# 通解

1. Postcondition
   ```text
   〈∀ j : α ≤ j < i : ¬Q.(f.j) 〉 ∧ (Q.(f.i) ∨ i = β-1)
   ```
2. Model the problem domain
   1. 定义C.i
      ```text
      * (0) C.i ≡ 〈∀ j : α ≤ j < i : ¬Q.(f.j) 〉, α ≤ i ≤ β-1
      ```
   2. 推出C.α
      ```text
      C.α
      = {(0)}
      〈∀ j : α ≤ j < α : ¬Q.(f.j) 〉
      = { Empty Range }
      Id∧

      (1) C.α ≡ True
      ```
   3. 推出C.(i+1)
      ```text
      C.(i+1)
      = {(0)}
      〈∀ j : α ≤ j < i+1 : ¬Q.(f.j) 〉
      = { Split off j = i term }
      〈∀ j : α ≤ j < i : ¬Q.(f.j) 〉 ∧ ¬Q.(f.i)
      = {(0)}
      C.i ∧ ¬Q.(f.i)

      (2) C.(i+1) ≡ C.i ∧ ¬Q.(f.i), α ≤ i < β-1
      ```
3. Rewrite the postcondition using the model.
   ```text
   Post : C.i ∧ (Q.(f.i) ∨ i = β-1)
   ```
4. Choose invariants.
   ```text
   P0: C.i
   P1: α ≤ i ≤ β-1
   ```
5. Guard
   ```text
   ¬Q.(f.i) ∧ i ≠ β-1
   ```
   bounded 的关键就在这里：除了没找到，还要保证没有到最后一个位置。
6. Establish invariants
   ```text
   i := α
   ```
7. Variant
   ```text
   (K ↓ β-1) - i
   ```
   where `K` is the smallest index such that `Q.(f.K)` is true, should such a place exist. If no such `K` exists, the bound `β-1` makes the loop stop.
8. Loop body
   ```text
   (i := i+1).P0
   = { Text Substitution }
   C.(i+1)
   = {(2)}
   C.i ∧ ¬Q.(f.i)
   = { P0 & guard }
   True
   ```
9. Finished algorithm
   ```text
   i := α
   ;do ¬Q.(f.i) ∧ i ≠ β-1 →
       i := i+1
   od

   ;if Q.(f.i) → "found case"
    [] ¬Q.(f.i) ∧ i = β-1 → "not found case"
    fi
   ```

# Two useful patterns

1. Does `Q` hold true everywhere?
   ```text
   〈∀ j : α ≤ j < i : Q.(f.j) 〉 ∧
   ((¬Q.(f.i) ∧ "no it doesn't")
    ∨
    (i = β-1 ∧ Q.(f.i) ∧ "yes it does"))
   ```
2. Does `Q` hold true anywhere?
   ```text
   〈∀ j : α ≤ j < i : ¬Q.(f.j) 〉 ∧
   ((Q.(f.i) ∧ "yes it does")
    ∨
    (i = β-1 ∧ ¬Q.(f.i) ∧ "no it doesn't"))
   ```
