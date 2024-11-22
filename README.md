# Group Testing Using Convex Optimization

This repository contains the implementation and analysis of a **Group Testing** strategy for identifying infected individuals in a large population. The approach leverages the sparsity of infections and solves a convex optimization problem to efficiently identify the infected subset of individuals using fewer tests than the population size.

---

## Problem Statement

In this problem, we will explore the idea of **group testing** as a strategy for testing a large population for a rare disease by pooling samples together. 

### Scenario

- **Population Size (`N`)**: We have a population of `N` people and collect saliva samples from each of them.  
- **Objective**: Identify infected individuals by detecting genetic signatures of a particular virus in their samples.  

### Assumptions

1. Let `x[n]` denote the concentration of the viral material in the sample for the `n`-th person:
   - For healthy individuals: `x[n] = 0`
   - For infected individuals: `x[n] > 0`
2. Our testing procedure:
   - Form a series of `M` mixtures of samples from different subsets of people.  
   - Run tests only on these mixtures.  
3. Goal: Use fewer tests (`M < N`) to identify the infected individuals.  

### Testing Approach

We construct mixtures of samples by creating random combinations, then attempt to recover the original vector `x` using convex optimization.

#### Sampling/Testing Process

1. Each of the `M` tests provides the concentration of viral material in the combined sample being tested.
2. Each person’s sample is divided into `K < M` equal portions, assigned at random to the `M` tests.
3. Representing the concentration of viral material in all mixed samples as a vector `y`:
   - `y` has dimensions of `M` (number of tests).
   - It is modeled as:  
     `y = A * x`  
     where:
     - `A`: A matrix of size `M x N`, representing the assignment of people to tests. Each column is constructed by randomly setting `K` entries to 1 (indicating inclusion in a test) and the remaining to 0.
     - `x`: The vector of concentrations for all individuals.

### Optimization Problem

Assume there is no noise in the tests, so `y` can be estimated perfectly. Given the matrix `A` and test results `y`, the inference problem is to estimate `x`. 

Although recovering `x` is generally impossible when `M < N`, it becomes feasible if `x` is sparse (most of the population is healthy). The sparsity of `x` allows us to solve the problem using the following convex optimization formulation:

- **Objective**: Minimize the 1-norm of `x` to promote sparsity (`sum(abs(x))`).
- **Constraints**:
  - `A * x = y` (consistency with test results)
  - `x >= 0` (concentration cannot be negative)

This formulation leverages advances in sparse recovery, which have been broadly appreciated only within the last 15 years.

---

## Implementation

### (a) Baseline Simulation

- **Setup:**
  - Population size: `N = 1000`
  - Number of tests: `M = 100`
  - Number of infected individuals: `10` (1% prevalence)
  - Each individual’s sample is split across `K = 10` tests.

- **Objective:**
  Verify that the approach identifies all 10 infected individuals.

- **Tools:**
  Implemented using Python and **CVXPY** for solving the convex optimization problem.

---

### (b) Experimenting with `K`

- Investigate the impact of the number of test divisions (`K`) on the accuracy of infection detection.
- **Objective:**
  Determine the minimum value of `K` for which the method reliably detects all infected individuals.

---

### (c) Disease Prevalence and `K`

- Simulate scenarios where the disease prevalence increases beyond 1%.
- **Objective:**
  Identify the maximum disease prevalence for which the method is effective and determine the optimal `K` value near the failure threshold.

