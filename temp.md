C.n=⟨↑j:30≤j<70:f.j⟩

Postcondition
<∀j:0<=j<i:odd(f.i)>∧((¬odd.(f.i) ∧ "no")∨(i = 199) ∧ odd.(f.i) ∧ “yes it does” ))

Domain model
`*(0) C.i = <∀j:0<=j<i:odd(f.i)> , 0<=i<200`

Consider
C.0
={(0)}
<∀j:0<=j<0:odd(f.i)>
={empty range}
true

`-(1) C.0 = true`

C.(i+1)
={(0)}
<∀j:0<=j<i+1:odd(f.i)>
={split off}
<∀j:0<=j<i:odd(f.i)> ∧ odd(f.i)
={(0)}
C.i ∧ odd(f.i)

`- (2) C.(i+1) ≡ C.i ∧ odd(f.i)`

Choose invariants. 
P0: C.i 
P1: 0 ≤ i ≤ 199

Guard
odd(f.i) ∧ i ≠ 199

Establish invariants. 
i := 0

Variant
(K↓199)−i


Postcondition
<∀i:0<=i<100: f.i <= f.k>
0<=k<100

Domain model
