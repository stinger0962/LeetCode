# Backtracking


###Definition by Wiki

   Backtracking is an algorithm for finding all (or some) solutions to some computational problems, notably constraint satisfaction problems.
  
   The algorithm **incrementally builds** candidates to the solutions, and **abandons** each partial candidate c ("backtracks") as soon as it determines that c cannot possibly be completed to a valid solution.


###Pseudocode


_denotions_

```
P : the data that the problem can be solved
c: candidate
first(P,c): first extension of c
next(P,s): next extension of s
```

_procedure bt(c)_
```
  if reject(P,c) then return
  if accept(P,c) then output(P,c)
  s ← first(P,c)
  while s ≠ Λ do
    bt(s)
    s ← next(P,s)
```