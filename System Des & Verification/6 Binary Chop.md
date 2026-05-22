> Example

Given `f[0..N]` of int, `1 ≤ N`, and `X : int`.

注意这里的范围是 `f[0..N]`，上界 `N` 是 included 的。

Precondition:

```text
f.0 ≤ X ∧ X < f.N
```

Postcondition:

```text
f.i ≤ X ∧ X < f.(i+1) ∧ 0 ≤ i < N
```

我们要找的是一个 pivot point：

```text
f.i ≤ X ∧ X < f.(i+1)
```

它表示 `i` 这个位置还在 `≤ X` 的一边，而 `i+1` 已经到了 `> X` 的一边。

为了做 loop，把 `i+1` 拆成一个新的变量 `j`：

```text
f.i ≤ X ∧ X < f.j ∧ j = i+1 ∧ 0 ≤ i < N
```

如果 `f` 是 ascending ordered，那么 pivot 是唯一的；如果还要判断 `X` 是否存在，只需要最后检查：

```text
f.i = X
```

这就是 Binary Search；而只找 pivot 的通用形状叫 Binary Chop。

统一格式：给定 ordered domain `[α..β]`，`α < β`，以及 boolean function `Q`。

`Q` 用来表示“当前位置还在左边 / true side”：

```text
Q.k ≡ k is still on the true side
```

在上面的例子里：

```text
Q.k ≡ f.k ≤ X
¬Q.k ≡ X < f.k
```

```text
Pre : Q.α ∧ ¬Q.β
Post: Q.i ∧ ¬Q.(i+1) ∧ α ≤ i < β
```

# 通解

1. Postcondition
   ```text
   Q.i ∧ ¬Q.(i+1) ∧ α ≤ i < β
   ```
2. Strengthen postcondition
   ```text
   Post : Q.i ∧ ¬Q.j ∧ j = i+1 ∧ α ≤ i < β
   ```
   引入 `j` 以后，loop 的目标变成不断缩小区间 `[i..j]`，直到 `j = i+1`。
3. Choose invariants.
   ```text
   P0: Q.i ∧ ¬Q.j
   P1: α ≤ i < j ≤ β
   ```
   `i` 保证在 true side，`j` 保证在 false side。
4. Establish invariants
   ```text
   i, j := α, β
   ```
   由 precondition `Q.α ∧ ¬Q.β` 可知初始成立。
5. Guard
   ```text
   j ≠ i+1
   ```
   如果 `j = i+1`，再结合 invariants，就得到 postcondition。
6. Variant
   ```text
   j - i
   ```
7. Loop body
   1. 选择中间点
      ```text
      h := (i+j) div 2
      ```
      guard `j ≠ i+1` 加上 `i < j` 可以保证：
      ```text
      i < h < j
      ```
      所以把 `i` 或 `j` 更新成 `h` 都会让 variant `j-i` 变小。
   2. Case `i := h`
      ```text
      (i := h).P0
      = { Text Substitution }
      Q.h ∧ ¬Q.j
      = { P0 }
      Q.h
      ```
      所以：
      ```text
      if Q.h → i := h
      ```
   3. Case `j := h`
      ```text
      (j := h).P0
      = { Text Substitution }
      Q.i ∧ ¬Q.h
      = { P0 }
      ¬Q.h
      ```
      所以：
      ```text
      if ¬Q.h → j := h
      ```
8. Finished algorithm
   ```text
   i, j := α, β
   ;do j ≠ i+1 →
       h := (i+j) div 2
      ;if Q.h → i := h
       [] ¬Q.h → j := h
       fi
   od
   ```

# Example algorithm

把 `Q.h` 换回 `f.h ≤ X`：

```text
i, j := 0, N
;do j ≠ i+1 →
    h := (i+j) div 2
   ;if f.h ≤ X → i := h
    [] X < f.h → j := h
    fi
od
```

# Binary Search

如果 `f` 是 ascending ordered，那么 pivot 唯一，`X` 只可能出现在 pivot 左边的 `i`。

```text
i, j := 0, N
;do j ≠ i+1 →
    h := (i+j) div 2
   ;if f.h ≤ X → i := h
    [] X < f.h → j := h
    fi
od
;Write (f.i = X)
```

为什么复杂度是 `O(log.N)`：每次选择中间点 `h := (i+j) div 2`，都会把候选区间大约减半。
