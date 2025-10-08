# Methodology: Bayesian Optimization with Gaussian Processes

## Table of Contents

1. [Introduction](#introduction)
2. [Theoretical Background](#theoretical-background)
3. [Algorithm Overview](#algorithm-overview)
4. [Implementation Details](#implementation-details)
5. [Acquisition Functions](#acquisition-functions)
6. [Hyperparameter Selection](#hyperparameter-selection)
7. [Experimental Setup](#experimental-setup)
8. [References](#references)

---

## Introduction

Bayesian Optimization is a sequential design strategy for global optimization of black-box functions that are expensive to evaluate. This methodology document provides a comprehensive explanation of the approach used in this project.

### Problem Statement

Given an unknown function \( f: \mathcal{X} \rightarrow \mathbb{R} \), where \( \mathcal{X} \subseteq \mathbb{R}^d \), we want to find:

\[
x^* = \arg\max_{x \in \mathcal{X}} f(x)
\]

Subject to constraints:
- **Limited evaluations**: We can only evaluate \( f \) a small number of times (budget-constrained)
- **No gradient information**: \( f \) is a black-box function
- **Potentially noisy**: Observations may contain noise

---

## Theoretical Background

### Gaussian Processes

A Gaussian Process (GP) is a collection of random variables, any finite number of which have a joint Gaussian distribution. It is fully specified by a mean function \( m(x) \) and a covariance (kernel) function \( k(x, x') \).

\[
f(x) \sim \mathcal{GP}(m(x), k(x, x'))
\]

**Properties**:
- Provides both **prediction** (mean) and **uncertainty** (variance)
- Non-parametric: Flexibility increases with data
- Principled way to incorporate prior knowledge through kernel choice

### Matérn Kernel

We use the Matérn kernel with \( \nu = 2.5 \):

\[
k(x, x') = \frac{2^{1-\nu}}{\Gamma(\nu)} \left(\sqrt{2\nu} \frac{||x - x'||}{l}\right)^\nu K_\nu\left(\sqrt{2\nu} \frac{||x - x'||}{l}\right)
\]

Where:
- \( l \): Length scale parameter (controls smoothness)
- \( \nu \): Smoothness parameter (fixed at 2.5)
- \( K_\nu \): Modified Bessel function

**Why Matérn 2.5?**
- Good balance between smoothness and flexibility
- Twice differentiable (suitable for most optimization problems)
- More robust than squared exponential kernel
- Computationally efficient

### Gaussian Process Regression

Given observations \( D = \{(x_i, y_i)\}_{i=1}^n \), the posterior distribution at a new point \( x_* \) is:

\[
f(x_*) | D \sim \mathcal{N}(\mu(x_*), \sigma^2(x_*))
\]

Where:

**Predictive Mean**:
\[
\mu(x_*) = k_*^T (K + \sigma_n^2 I)^{-1} y
\]

**Predictive Variance**:
\[
\sigma^2(x_*) = k(x_*, x_*) - k_*^T (K + \sigma_n^2 I)^{-1} k_*
\]

Where:
- \( K \): Kernel matrix on training points
- \( k_* \): Kernel vector between \( x_* \) and training points
- \( \sigma_n^2 \): Noise variance
- \( y \): Vector of observed outputs

---

## Algorithm Overview

### Sequential Bayesian Optimization

```
1. Initialize with initial data D = {(x_i, y_i)}
2. Repeat until budget exhausted:
   a. Fit GP model to current data D
   b. Optimize acquisition function to find x_next
   c. Evaluate f(x_next) → y_next
   d. Augment data: D ← D ∪ {(x_next, y_next)}
3. Return x_best with highest observed y value
```

### Key Components

1. **Surrogate Model**: Gaussian Process approximates the unknown function
2. **Acquisition Function**: Determines where to sample next
3. **Optimization**: Finds the point that maximizes the acquisition function
4. **Update**: Incorporates new observation and repeats

---

## Implementation Details

### Data Preprocessing

**Input Normalization**:
```python
# Normalize inputs to [0, 1] range
X_normalized = (X - X.min(axis=0)) / (X.max(axis=0) - X.min(axis=0))
```

**Output Standardization**:
```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
y_scaled = scaler.fit_transform(y.reshape(-1, 1)).ravel()
```

**Benefits**:
- Improves GP numerical stability
- Makes kernel hyperparameters more interpretable
- Facilitates comparison across different functions

### Gaussian Process Configuration

```python
from sklearn.gaussian_process import GaussianProcessRegressor
from sklearn.gaussian_process.kernels import Matern

kernel = Matern(nu=2.5, length_scale=1.0, length_scale_bounds=(1e-5, 1e5))

gp = GaussianProcessRegressor(
    kernel=kernel,
    n_restarts_optimizer=10,  # Multiple restarts for hyperparameter optimization
    alpha=1e-6,               # Noise level (jitter for numerical stability)
    normalize_y=True,         # Normalize outputs internally
    random_state=SEED
)
```

### Hyperparameter Optimization

The GP kernel hyperparameters are optimized via Maximum Likelihood Estimation:

\[
\theta^* = \arg\max_\theta \log p(y | X, \theta)
\]

Where the log marginal likelihood is:

\[
\log p(y | X, \theta) = -\frac{1}{2} y^T K_\theta^{-1} y - \frac{1}{2} \log |K_\theta| - \frac{n}{2} \log 2\pi
\]

---

## Acquisition Functions

Acquisition functions balance **exploration** (sampling uncertain regions) vs **exploitation** (sampling near known good points).

### 1. Expected Improvement (EI)

**Formula**:
\[
\text{EI}(x) = \mathbb{E}[\max(f(x) - f(x^+), 0)]
\]

Where \( f(x^+) \) is the current best observation.

**Analytical Form**:
\[
\text{EI}(x) = \begin{cases}
(\mu(x) - f(x^+) - \xi) \Phi(Z) + \sigma(x) \phi(Z) & \text{if } \sigma(x) > 0 \\
0 & \text{if } \sigma(x) = 0
\end{cases}
\]

Where:
- \( Z = \frac{\mu(x) - f(x^+) - \xi}{\sigma(x)} \)
- \( \Phi \): Standard normal CDF
- \( \phi \): Standard normal PDF
- \( \xi \): Exploration parameter (default: 0.01)

**Implementation**:
```python
def expected_improvement(self, X, xi=0.01):
    mu, sigma = self.gp.predict(X, return_std=True)
    mu_sample_opt = np.max(self.y_scaled)
    
    with np.errstate(divide='warn'):
        imp = mu - mu_sample_opt - xi
        Z = imp / sigma
        ei = imp * norm.cdf(Z) + sigma * norm.pdf(Z)
        ei[sigma == 0.0] = 0.0
    
    return ei
```

**Characteristics**:
- ✅ Smooth and differentiable
- ✅ Good for exploitation
- ✅ Well-studied and reliable
- ⚠️ Can be overly conservative in exploration

### 2. Upper Confidence Bound (UCB)

**Formula**:
\[
\text{UCB}(x) = \mu(x) + \kappa \cdot \sigma(x)
\]

Where \( \kappa \) controls the exploration-exploitation trade-off.

**Implementation**:
```python
def upper_confidence_bound(self, X, kappa=2.576):
    mu, sigma = self.gp.predict(X, return_std=True)
    return mu + kappa * sigma
```

**Characteristics**:
- ✅ Simple and intuitive
- ✅ Good for exploration
- ✅ Theoretically grounded (regret bounds)
- ⚠️ Requires careful tuning of \( \kappa \)

**Kappa Selection**:
- \( \kappa = 2.0 \): Moderate exploration
- \( \kappa = 2.576 \): 99% confidence interval (default)
- \( \kappa = 3.0 \): Aggressive exploration

### Acquisition Function Optimization

To find the next point to sample:

\[
x_{\text{next}} = \arg\max_{x \in \mathcal{X}} \alpha(x)
\]

**Implementation Strategy**:
1. Generate random candidate points (e.g., 10,000)
2. Evaluate acquisition function at all candidates
3. Select point with maximum acquisition value

```python
def optimize_acquisition(self, acquisition_func='ei', n_candidates=10000):
    # Generate random candidates
    X_candidates = np.random.uniform(0, 1, size=(n_candidates, self.n_dimensions))
    
    # Evaluate acquisition function
    if acquisition_func == 'ei':
        acq_values = self.expected_improvement(X_candidates)
    elif acquisition_func == 'ucb':
        acq_values = self.upper_confidence_bound(X_candidates)
    
    # Return best candidate
    best_idx = np.argmax(acq_values)
    return X_candidates[best_idx]
```

**Alternative**: Multi-start gradient-based optimization (L-BFGS-B) for more refined solutions.

---

## Hyperparameter Selection

### Acquisition Function Selection

**Decision Rules**:

1. **Expected Improvement (EI)** when:
   - Function is relatively smooth
   - Need stable convergence
   - Low to medium dimensionality (≤ 5D)

2. **Upper Confidence Bound (UCB)** when:
   - Function has high uncertainty
   - Need more exploration
   - Multi-modal landscape suspected
   - Higher dimensionality (> 5D)

### Exploration Parameters

**Expected Improvement \( \xi \)**:
- \( \xi = 0.0 \): Pure exploitation
- \( \xi = 0.01 \): Default balanced approach
- \( \xi = 0.1 \): More exploration

**UCB \( \kappa \)**:
- Adaptive strategy based on iteration:
  \[
  \kappa(t) = \sqrt{2 \log(t \cdot d^2 \cdot \pi^2 / 6\delta)}
  \]
  Where \( t \) is iteration number, \( d \) is dimensionality, \( \delta \) is confidence parameter.

### GP Kernel Hyperparameters

**Length Scale Bounds**: `(1e-5, 1e5)`
- Allows both very local and very global correlations
- Automatically optimized via MLE

**Noise Level (alpha)**: `1e-6`
- Small value for numerical stability
- Can be increased if function evaluations are noisy

---

## Experimental Setup

### Test Functions

Eight functions with varying properties:

| Function | Dim | Samples | Characteristics |
|----------|-----|---------|-----------------|
| 1        | 2   | 10      | Simple, low-dimensional |
| 2        | 2   | 10      | Simple, low-dimensional |
| 3        | 3   | 15      | Moderate complexity |
| 4        | 4   | 30      | Higher dimensional |
| 5        | 4   | 20      | Higher dimensional |
| 6        | 5   | 20      | High dimensional |
| 7        | 6   | 30      | High dimensional |
| 8        | 8   | 40      | Very high dimensional |

### Evaluation Budget

- Limited number of additional function evaluations
- Must balance exploration and exploitation within budget
- Budget varies by function complexity

### Performance Metrics

1. **Best Value Found**: \( \max_{i} y_i \)
2. **Simple Regret**: \( r_t = f(x^*) - \max_{i \leq t} f(x_i) \)
3. **Cumulative Regret**: \( R_t = \sum_{i=1}^t [f(x^*) - f(x_i)] \)

### Reproducibility

- Fixed random seeds for all stochastic components
- Seed derived from base seed: `SEED = 42`
- Function-specific seeds: `SEED + function_id`

```python
np.random.seed(SEED)
gp = GaussianProcessRegressor(..., random_state=SEED)
```

---

## Advantages and Limitations

### Advantages

✅ **Sample Efficient**: Finds good solutions with few evaluations  
✅ **Uncertainty Quantification**: Provides confidence in predictions  
✅ **Principled**: Based on solid statistical theory  
✅ **Flexible**: Applicable to many optimization problems  
✅ **No Gradients Required**: Works with black-box functions  

### Limitations

⚠️ **Computational Cost**: O(n³) scaling with number of samples  
⚠️ **Dimensionality**: Struggles beyond 20-30 dimensions  
⚠️ **Assumptions**: Assumes function can be modeled by GP  
⚠️ **Local Optima**: May converge to local optima in multi-modal landscapes  
⚠️ **Hyperparameter Sensitivity**: Performance depends on kernel choice  

---

## References

### Key Papers

1. **Mockus, J. (1975)**. "On Bayesian Methods for Seeking the Extremum." *Optimization Techniques IFIP Technical Conference*.

2. **Jones, D. R., Schonlau, M., & Welch, W. J. (1998)**. "Efficient Global Optimization of Expensive Black-Box Functions." *Journal of Global Optimization*, 13(4), 455-492.

3. **Snoek, J., Larochelle, H., & Adams, R. P. (2012)**. "Practical Bayesian Optimization of Machine Learning Algorithms." *Advances in Neural Information Processing Systems*, 25.

4. **Rasmussen, C. E., & Williams, C. K. I. (2006)**. *Gaussian Processes for Machine Learning*. MIT Press.

5. **Shahriari, B., Swersky, K., Wang, Z., Adams, R. P., & de Freitas, N. (2016)**. "Taking the Human Out of the Loop: A Review of Bayesian Optimization." *Proceedings of the IEEE*, 104(1), 148-175.

### Software Libraries

- **Scikit-learn**: Gaussian Process implementation
- **GPy**: Advanced GP library
- **GPyOpt**: Dedicated Bayesian Optimization library
- **BoTorch**: PyTorch-based Bayesian Optimization
- **Ax**: Adaptive experimentation platform

---

**Document Version**: 1.0  
**Last Updated**: October 8, 2025  
**Author**: Capstone Project Team

