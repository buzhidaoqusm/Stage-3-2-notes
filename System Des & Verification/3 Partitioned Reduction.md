> Example

Count the number of even values in `f[0..N)` of int.
Compute the sum of values which satisfy some predicate `Q`.
Determine the largest value among the elements which satisfy `Q`.

统一格式：先用 `Q` 把范围分成两部分；满足 `Q` 的元素贡献 `h.(f.j)`，不满足 `Q` 的元素贡献 `Id⊕`

`r = 〈⊕ j : α≤ j <β : g.(f.j) 〉`

where

- `g.x = h.x ⇐ Q.x`
- `g.x = Id⊕ ⇐ ¬Q.x`

具体例子：统计偶数个数时

- `⊕` 是 `+`
- `h.x = 1`
- `Q.x = even.x`
- `Id+ = 0`
- `g.x = 1 ⇐ even.x`
- `g.x = 0 ⇐ odd.x`

# 通解

1. Postcondition
   - `r = 〈⊕ j : α ≤ j < β : g.(f.j) 〉`
2. Model the problem domain
   1. 定义C.n
      ```text
      * (0) C.n = 〈⊕ j : α ≤ j < n : g.(f.j) 〉, α ≤ n ≤ β
      ```
   2. 定义g.x
      ```text
      * (1) g.x = h.x ⇐ Q.x
      * (2) g.x = Id⊕ ⇐ ¬Q.x
      ```
   3. 推出C.α
      ```text
      C.α
      = {(0)}
      〈⊕ j : α≤ j < α : g.(f.j) 〉
      = { Empty Range }
      Id⊕
      ```
   4. 推出满足Q时的C.(n+1)
      ```text
      C.(n+1)
      = {(0)}
      〈⊕ j : α ≤ j < n+1 : g.(f.j) 〉
      = { Split off j = n term }
      〈⊕ j : α ≤ j < n : g.(f.j) 〉 ⊕ g.(f.n)
      = { Case Q.(f.n), (1) }
      〈⊕ j : α≤ j < n : g.(f.j) 〉 ⊕ h.(f.n)
      = {(0)}
      C.n ⊕ h.(f.n)

      (4) C.(n+1) = C.n ⊕ h.(f.n) ⇐ Q.(f.n), α≤ n <β
      ```
   5. 推出不满足Q时的C.(n+1)
      ```text
      C.(n+1)
      = {(0)}
      〈⊕ j : α≤ j < n+1 : g.(f.j) 〉
      = { Split off j = n term }
      〈⊕ j : α≤ j < n : g.(f.j) 〉 ⊕ g.(f.n)
      = { Case ¬Q.(f.n), (2) }
      〈⊕ j : α≤ j < n : g.(f.j) 〉 ⊕ Id⊕
      = {(0)}
      C.n ⊕ Id⊕

      (5) C.(n+1) = C.n ⊕ Id⊕ ⇐ ¬Q.(f.n), α≤ n <β
      ```
3. Rewrite the postcondition using the model.
   - `Post : r = C.β`
4. Strengthen the postcondition
   - `Post : r = C.n ∧ n = β`
5. Choose invariants.
   - `P0: r = C.n`
   - `P1: α ≤ n ≤ β`
6. Guard
   - `n ≠ β`
7. Establish invariants
   - `n, r := α, Id⊕`
8. Variant
   - `β - n`
9. Loop body
   1. Case `Q.(f.n)`
      ```text
      (n, r := n+1, E).P0
      = { Text Substitution }
      E = C.(n+1)
      = { Case Q.(f.n), (4) }
      E = C.n ⊕ h.(f.n)
      = {P0}
      E = r ⊕ h.(f.n)
      ```
   2. Case `¬Q.(f.n)`
      ```text
      (n, r := n+1, E).P0
      = { Text Substitution }
      E = C.(n+1)
      = { Case ¬Q.(f.n), (5) }
      E = C.n ⊕ Id⊕
      = {P0}
      E = r ⊕ Id⊕
      ```
10. Finished algorithm
    ```text
    n, r := α, Id⊕
    ;do n ≠β→
        if Q.(f.n) → n, r := n+1, r ⊕ h.(f.n)
        [] ¬Q.(f.n) → n, r := n+1, r ⊕ Id⊕
        fi
    od
    ```
