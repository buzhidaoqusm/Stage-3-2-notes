> Example

Given an array `f[0..N)` of int and a target value `X`.
Find the smallest index `i` such that `f.i = X`.

Precondition: `X` is present in `f`.

```text
Pre : 〈∃ j : 0 ≤ j < N : f.j = X 〉
Post: 〈∀ j : 0 ≤ j < i : f.j ≠ X 〉 ∧ f.i = X
```

为什么post前面要有 `〈∀ j : 0 ≤ j < i : f.j ≠ X 〉` : 因为是线性搜索，找到的是第一个X，所以必须确保X前面没有等于X的值

postcondition是最强的条件，invariant + guard 为假 = postcondition

统一格式：在有序、非空的范围 `f[α..β)` 中，找到第一个满足 predicate `Q` 的位置。

`Q` 是定义在元素上的 predicate，用来描述我们要找的性质：

```text
Q.x ≡ element x satisfies the property we are searching for
Q.(f.i) ≡ f.i satisfies Q
```

在这个例子里，`Q` 就是“元素等于目标值 `X`”：

```text
Q.x ≡ x = X
Q.(f.i) ≡ f.i = X
```

```text
Pre : 〈∃ j : α ≤ j < β : Q.(f.j) 〉
Post: 〈∀ j : α ≤ j < i : ¬Q.(f.j) 〉 ∧ Q.(f.i)
```

写成 `Post: 〈∀ j : 0 ≤ j < i : f.j ≠ X 〉 ∧ f.i = X` 也行，Q是通解的写法

# 通解

1. Postcondition
   ```text
   〈∀ j : α ≤ j < i : ¬Q.(f.j) 〉 ∧ Q.(f.i)
   ```
2. Model the problem domain
   1. 定义C.i (C.i要确保每次循环的时候都为真，所以不能加上 `Q.(f.i)`，而是把它放在guard)
      ```text
      * (0) C.i ≡ 〈∀ j : α ≤ j < i : ¬Q.(f.j) 〉, α ≤ i ≤ β
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

      (2) C.(i+1) ≡ C.i ∧ ¬Q.(f.i), α ≤ i < β
      ```
3. Rewrite the postcondition using the model.
   ```text
   Post : C.i ∧ Q.(f.i)
   ```
4. Choose invariants.
   ```text
   P0: C.i
   P1: α ≤ i ≤ β
   ```
5. Guard
   ```text
   ¬Q.(f.i)
   ```
6. Establish invariants
   ```text
   i := α
   ```
7. Variant
   ```text
   K - i
   ```
   where `K` is the smallest index such that `Q.(f.K)` is true.
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
   ;do ¬Q.(f.i) →
       i := i+1
   od
   ```
