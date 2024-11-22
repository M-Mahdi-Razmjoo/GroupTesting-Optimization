# Group Testing Using Convex Optimization

This repository contains the implementation and analysis of a **Group Testing** strategy for identifying infected individuals in a large population. The approach leverages the sparsity of infections and solves a convex optimization problem to efficiently identify the infected subset of individuals using fewer tests than the population size.

---

## Problem Statement

We are testing a population of size $ N $ for a rare disease by pooling saliva samples and testing the mixtures. The challenge is to identify the infected individuals ($ x_n > 0 $) from $ M $ tests, where $ M < N $, using convex optimization.

### Mathematical Representation

1. **Testing Process**  
   The concentration of viral material in the $ m $-th test is given by:
   $
   y = Ax
   $
   where:
   - $ y \in \mathbb{R}^M $: vector of test results
   - $ A \in \mathbb{R}^{M \times N} $: mixing matrix where $ A_{ij} $ indicates whether the $ j $-th individual contributed to the $ i $-th test
   - $ x \in \mathbb{R}^N $: vector of concentrations for each individual

2. **Optimization Problem**  
   To recover the sparse $ x $, solve the following convex optimization problem:
   $
   \min_x \|x\|_1 \quad \text{subject to} \quad Ax = y, \, x \geq 0
   $

---

## Implementation

### (a) Baseline Simulation

- **Setup:**
  - Population size: $ N = 1000$
  - Number of tests: $ M = 100 $
  - Number of infected individuals: $ 10 $ (1% prevalence)
  - Each individual’s sample is split across $ K = 10 $ tests.

- **Objective:**
  Verify that the approach identifies all $ 10 $ infected individuals.

- **Tools:**  
  Implemented using Python and **CVXPY** for solving the convex optimization problem.

---

### (b) Experimenting with $ K $

- Investigate the impact of the number of test divisions $ K $ on the accuracy of infection detection.
- **Objective:**
  Determine the minimum $ K $ value where the algorithm remains effective.

---

### (c) Disease Prevalence and $ K $

- Simulate scenarios where the prevalence of the disease increases beyond 1%.
- **Objective:**
  Identify the maximum disease prevalence for which the approach works and determine the optimal $ K $ value near the failure threshold.

---

## Results

1. **Baseline Case:**
   - Successfully identified all infected individuals with $ K = 10 $.

2. **Effect of $ K $:**
   - Found the threshold value of $ K $ where accuracy starts to decline.

3. **Increasing Prevalence:**
   - Identified the prevalence limit for the method and the corresponding optimal $ K $.


