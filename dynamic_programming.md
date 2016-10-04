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


###Two-dimensional DP Table

During the course of practice, I've observed several rules regarding 2D DP table. 

1. **Be clear**. The first step is to make clear what **each cell represents**, and how/if it contributes to the final solution. A wrong choice can be misleading and it is easy to make such a mistake in some scenarios.
2. **Be flexible.** If an attempt to build the table from top-left to right-bottom (**bottom-up**) seems unfeasible, try to do it in the opposite direction (**top down**). 
3. **Be familiar with patterns**. Value in ```DP[i][j]``` is usually a combination of its **vertical and horizontal neighbors**.(i.e. ```DP[i-1][j] and DP[i][j-1]```, depending on the direction of building) 
But in many times, it is also has something to do with its **diagonal neighbor**.(i.e. ```DP[i-1][j-1]```)
