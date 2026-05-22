> Example

Compute the sum of the values in `f[0..N)` of int.
Compute the product
Determine the largest value

统一格式：`⊕` 可以是 +（求和），* （求积），↑ （最大值）
`r = 〈⊕ j : α≤ j <β : f.j 〉`

# 通解
1. Postcondition
   ```text
   r = 〈⊕ j : α≤ j <β : f.j 〉
   ```
2. Model the problem domain
   1. 定义C.n
      ```text
      * (0) C.n = 〈⊕ j : α ≤ j < n : f.j 〉, α ≤ n ≤ β
      ```
   2. 推出C.α
      ```text
      C.α
      = {(0)}
      〈⊕ j : α ≤ j < α : f.j 〉
      = { Empty Range }
      Id⊕
      ```
   3. 推出C.(n+1)
      ```text
      C.(n+1)
      = {(0)}
      〈⊕ j : α≤ j < n+1 : f.j 〉
      = { Split j = n term }
      〈⊕ j : α≤ j < n : f.j 〉 ⊕ f.n
      = {(0)}
      C.n ⊕ f.n

      (2) C.(n+1) = C.n ⊕ f.n, α ≤ n < β
      ```
3. Rewrite the postcondition using the model.
   ```text
   Post : r = C.β
   ```
4. Strengthen the postcondition
   ```text
   Post : r = C.n ∧ n = β
   ```
5. Choose invariants.
   ```text
   P0: r = C.n
   P1: α ≤ n ≤ β
   ```
6. Guard
   ```text
   n ≠ β
   ```
7. Establish invariants
   ```text
   n, r := α, Id⊕
   ```
8. Variant
   ```text
   β - n
   ```
9. Loop body
   ```text
   (n, r := n+1, E).P0
   = { Text Substitution }
   E = C.(n+1)
   = {(2)}
   E = C.n ⊕ f.n
   = {P0}
   E = r ⊕ f.n
   ```
10. Finished algorithm
    ```text
    n, r := α, Id⊕
    ;do n ≠β→
        n, r := n+1, r ⊕ f.n
    od
    ```
