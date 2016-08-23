# Combination Sum I, II, III


> This article is about a series of three problems.

## Combination Sum I

Given a set of candidate numbers (**C**) and a target number (**T**), find all unique combinations in **C** where the candidate numbers sums to **T**.

The **same** repeated number may be chosen from **C** unlimited number of times.

Note:
All numbers (including target) will be **positive** integers.
The solution set must not contain duplicate combinations.
For example, given candidate set``` [2, 3, 6, 7]``` and target ```7```, 
A solution set is: 
```
[
  [7],
  [2, 2, 3]
]```




---


### Find All Solutions, Try Backtracking

This is a typical find-all-solutions problem. We build the solution set from all possible routes and abandon a route when it becomes invalid. 

###When to abandon?

We notice that all numbers, including the target are **positive** numbers. Such restriction allows us to add up all the candidates as we build the solution, and abondon the last candidate(backtrack) when the sum exceeds the target.

###Avoid duplicate solution

To avoid duplicate solutions, we need to introduce an additional parameter which records the starting position of each loop. The starting index **ONLY** moves forward.
