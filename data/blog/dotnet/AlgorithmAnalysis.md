---
title: Algorithm Analsis
date: '2021-10-02'
tags: ['.net', 'datastructure']
draft: false
summary: Big O Notation
images: []
layout: PostLayout
---

## Algorithm Analysis

> What is an Algorithm?

The science of computing is concerned with usig computer to solve problems.

An algorithm is a set of well-defined rules for solving a computational problem.

Algorith analysis is concerned with comparing algorithms based upon the amount of computing resources that each algorithm uses.

Computing resource can be examined terms of:

- **Space Requirements** i.e the amount of space or memory an algorithm requires t solve the problem.

- **Execution/Running time** the length of time an algorith requires to exucute.

Benchmark Analysis tracking the time use to executet the function.

- Experimental running times of two algorithms are difficult to directly compare unless the experiments are performed in the same hardware and software environments.

- Experiments can be done only on a limited set of test inputs: hence they leave out the running times of inputs not included in the experiment ( and thes inputs may be important)

- An algorithm must be fully implemented in order to execute it to study its running time experimentally.

### Big-0 Notation

### Time-Complexity

Big 0 time is the language and metric we use to describe the efficency of algorithms.

To analyze the running time of an algorithm without performing experiments, we perform an analysis directly on a high-level description of the algorithm(Pseudocode).
pdate-corona.herokuapp.com
We can adopt one of the three strategies:

- **Best case Analysis** - we consider the specific input that results in the execution of the fewest possible primitive operation.

- **Worst-Case Analysis** - We consider the specific input that result in the execution of the maximum possible primitive operations.

- **Average Case Analysis** - We try to determine the average number of primitive operations executed for all possible inputs of a given size.

1. It turns out that the exact number of operations is not as important as determing the most dominant part of the function.
2. The order of magnitude function describe the part of the original functions T(n) that increases the fastets as the value on n increases.
3. T(n) = 1 + n. As n gets large, the constant 1 will become less and less significatn to the final result.
4. The big-0 notation allows us to ignore constant factors and lower-order terms oand focus on the main components of a function that affects its growth
5. The function f(n) providse a simple representation of the domainant part of the original T(n).
6. F(n) =O(n), Big O is order of magnitude function that describe the dominant part of the function, main component of the function that affects the growth.

### Common Big-0 Functions

- F(n) - Name
- 1 - Constant
- log n - Logarithmic
- n - Linear
- n log n - Log Linear
- n2 - Quadratic
- n3 - Cubic
- 2n - Exponential

### Space Complexity

The space complexity of an algorithm is the amount of additional or auxiliary memory space that the algorithm requires.

### Examples

The loop will run n times. The higher the n the higher the order of magnitude. The runtime is **Linear**

```python
for x in range(n):
    # Do something
```

The big O is **O(N)**.

When we look at big O ,we will look at the most dominant part that will influence the computation of the function even if the function is skiping 2 numbers in each loop.

```python
for x in range(1, n, 2):
    # Do someting
```

The big O is **O(n)**.
