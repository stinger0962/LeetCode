# Dynamic Programming



###Introdution

Dynamic programming is a method for solving a complex problem by breaking it down into a collection of simpler **subproblems**, solving each subproblem once, and storing the solution.


###Dynamic Programming and Recursion

Both methods break down a complex problem into subproblems, yet their approaches are different.

**Dynamic Programming -- Bottom Up: ** DP is usually based on a recurrent formula and one (or some) starting states. DP start solving from the trivial subproblem, up towards the given problem.
```
(e.g. of DP) Given formular f(n) = max (f(n-1), f(n-2)+1 );
             Starting states : f(1) = 2, f(2) = 3;     
             Then we loop from 3 to n, 
               Finding all the subproblems towards n 
```

**Recursion -- Top down**: Recursion does the same job in the opposite direction. Given an ending states, and a relationship to its subproblem, it begin with core(main) problem then breaks it into subproblems and solve these subproblems similarily.

```
(e.g. of Recursion) RecurFunction(para n)
                          if(n == 1) // Ending condition
                            do ...
                          RecurFunction(para n-1) 
```

In **recursion** some subproblems may occur multiple times, uncessary subproblems are not solved.

In **Dynamic Programming**, each subproblem is solved once no matter if it's necessary or not.
  
