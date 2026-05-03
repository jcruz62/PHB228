# PHB 228: Statistical Computing
## Homework 1: R Fundamentals, Matrix Algebra, and Simulation
**Assigned:** Thursday, April 16, 2026 
**Due:** Sunday, May 3, 2026, 11:59 PM
**Points:** 100

## Overview

This assignment covers Lectures 1-9: vectorization, version
control, data structures, functional programming, matrix
operations, optimization, simulation, and bootstrap methods.

## Technical Requirements

- R (version 4.4.0 or later) and RStudio Desktop
- Git and GitHub account
- Required R packages: `tidyverse`, `bench`, `palmerpenguins`,
  `purrr`

**Note:** Base R plotting functions are acceptable for all
visualizations in this assignment. ggplot2 will be taught formally
in Lectures 14-15; you are welcome to use it if you are already
comfortable with it, but it is not required.

## Submission Guidelines

- Submit a Quarto file (`.qmd`) with your code and explanations
- Submissions that fail to produce a rendered output file will not be graded.
- Render with: `quarto render hw1_[your_PID].qmd`
- Push all files to your personal GitHub repository and add the TA, Man Luo (GitHub account @Aileen-Luo), as a collaborator.
- Late submissions: 10% penalty per day, up to 3 days

---

## Part 1: Vectorization and Data Structures (15 points)

### 1.1 Vectorization (8 points)

Using the built-in `iris` dataset:

a) (2 points) Create a function `vectorized_zscore` that
standardizes a numeric vector using vectorized operations
(z = (x - mean(x)) / sd(x)). Create a second function
`loop_zscore` that performs the same computation using a `for`
loop.

b) (3 points) Use `bench::mark()` to compare performance with at
least 50 iterations. Create a bar plot comparing median
execution times.

c) (3 points) Using the `ChickWeight` dataset, create a
`Growth.Rate` column (weight change per day for each chick) and
a `Weight.Gain.Percent` column (percentage gain from first to
last measurement). Calculate average growth rate and mean maximum
weight by diet group.

### 1.2 Data Structures and Functional Programming (7 points)

Using the `palmerpenguins` dataset:

a) (2 points) Extract the unique species and create a list where
each element contains data for one species. Add a sample size
attribute to each element.

b) (1 point) Use `lapply()` to calculate the mean of each
numeric variable (excluding NAs). Redo using `map_dbl()`.

c) (1 point) Use `tapply()` to find the mean body mass by
species and by the combination of species and sex.

d) (1 point) Demonstrate R's copy-on-modify semantics: create a
vector, create a reference to it, modify the reference, and
explain what happens (2-3 sentences).

e) (2 points) Rewrite the following inefficient code using
preallocation. Compare execution times with `system.time()` and
explain why your version is faster (2-3 sentences).

```r
result <- numeric(0)
for (i in 1:10000) {
  result <- c(result, i^2)
}
```

---

## Part 2: Matrix Operations and Decompositions (30 points)

### 2.1 Matrix Operations (8 points)

Consider the following matrices:

```r
A <- matrix(c(4, 2, 1, 5), nrow = 2)
B <- matrix(c(3, 1, 0, 2), nrow = 2)
C <- matrix(c(1, 3, 5, 2, 4, 6), nrow = 2)
```

a) (4 points) Calculate and explain the operation performed in
each case: `A + B`, `A * B`, `A %*% B`, `t(A) %*% C`,
`solve(A)`.

b) (4 points) Create a function `is_symmetric` that returns TRUE
if a matrix is symmetric and FALSE otherwise. Print an error if
the input is not square. Test on A, B, and `t(A) %*% A`.

### 2.2 Eigenvalue Decomposition (10 points)

```r
M <- matrix(c(4, 2, 2, 3), nrow = 2)
```

a) (3 points) Compute eigenvalues and eigenvectors using
`eigen()`.

b) (3 points) Verify the eigenvector equation Mx = lx holds for
each pair.

c) (4 points) Reconstruct M using M = PDP^{-1} and verify it
matches the original.

### 2.3 SVD and QR Decomposition (12 points)

a) **SVD** (6 points): Compute the SVD of the matrix below.
Create a rank-1 approximation, calculate the Frobenius norm of
the difference, and explain what information is lost.

```r
X <- matrix(c(3, 2, 2, 2, 3, -2, 2, -2, 3), nrow = 3)
```

b) **QR and Least Squares** (6 points): Using the data below,
create the design matrix, solve the least squares problem via QR
decomposition, compare with `lm()`, and explain the advantages
of QR decomposition for least squares.

```r
set.seed(228)
n <- 100
x <- runif(n, -5, 5)
y <- 3 + 2 * x + rnorm(n, sd = 1)
```

---

## Part 3: Optimization and Simulation (40 points)

### 3.1 Gradient Descent (15 points)

Minimize the Rosenbrock function:
f(x,y) = (1-x)^2 + 100(y-x^2)^2

a) (3 points) Write `rosenbrock(x)` that takes a vector
x = c(x1, x2) and returns the function value.

b) (5 points) Write `rosenbrock_grad(x)` that returns the
gradient vector.

c) (4 points) Implement `gradient_descent(init, grad_fn,
learning_rate, max_iter, tol)` that returns the final solution,
function value, and iteration count. Stop when the Euclidean
norm of the gradient is less than the tolerance.

d) (3 points) Apply from starting point (-1.2, 1.0) with
learning_rate = 0.001, max_iter = 10000, tol = 1e-6. Report
the solution, function value, and iterations.

### 3.2 Comparison with Built-in Methods (10 points)

a) (5 points) Use `optim()` with "BFGS" to minimize the
Rosenbrock function from the same starting point.

b) (5 points) Use `optim()` with "Nelder-Mead". Compare all three
methods (your gradient descent, BFGS, Nelder-Mead) in terms of
solution accuracy, iterations, and function evaluations.

### 3.3 Simulation Study (15 points)

Compare the mean, median, and 10% trimmed mean as estimators of
central tendency.

a) (7 points) Write `run_simulation(n_samples, n_reps,
dist_type)` that generates `n_reps` samples of size `n_samples`,
computes all three estimators, and returns a data frame.

b) (4 points) Run with n = 30, 1000 replications, under three
distributions: normal(0,1), t(df=3), and contaminated normal
(90% from N(0,1), 10% from N(0,10)). Calculate bias, variance,
and MSE for each estimator under each distribution.

c) (4 points) Create a summary table and write a brief paragraph
(3-5 sentences) interpreting which estimator performs best under
which conditions and why.

---

## Part 4: Bootstrap Methods (15 points)

### 4.1 Bootstrap Confidence Intervals (8 points)

a) (4 points) Implement `bootstrap_mean()` that estimates the
standard error and 95% confidence interval for the mean using
the nonparametric bootstrap. Support three CI types:
"percentile", "basic", and "normal". Apply to the returns data
below and report all three intervals.

```r
set.seed(42)
returns <- rnorm(50, mean = 0.08, sd = 0.12)
returns <- returns + rexp(50, rate = 20) - 0.05
```

b) (4 points) Create a histogram of the bootstrap distribution
of the mean with vertical lines indicating the three different
confidence intervals. Discuss which CI method is most
appropriate for this data and why (3-5 sentences).

### 4.2 Bootstrap Hypothesis Testing (7 points)

a) (4 points) Implement `bootstrap_ttest()` that performs a
bootstrap test for the difference in means between two groups.
Apply to the data below with 2000 bootstrap samples. Create a
visualization showing the bootstrap null distribution with the
observed difference marked.

```r
set.seed(123)
control <- rnorm(40, mean = 10, sd = 2)
treatment <- rnorm(40, mean = 11.5, sd = 2)
```

b) (3 points) Compare your bootstrap test results with a standard
t-test. Discuss any differences and potential advantages of the
bootstrap approach (3-5 sentences).

---

## Grading Rubric

| Component | Points |
|-----------|--------|
| Vectorization and Data Structures | 15 |
| Matrix Operations and Decompositions | 30 |
| Optimization and Simulation | 40 |
| Bootstrap Methods | 15 |
| **Total** | **100** |

Additional criteria applied throughout:

- Code quality: clear, well-commented, efficient implementations
- Visualizations: proper labels, titles, and legends
- Written explanations: concise, accurate, and insightful


## Academic Integrity

Your submission must be your own work. You may discuss concepts
with classmates, but all code and analysis must be completed
individually. Cite any resources used, including AI tools with
the explicit prompts that were used.
