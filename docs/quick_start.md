# Quick Start Guide

Get started with Bayesian Optimization in 5 minutes!

## Prerequisites

Make sure you've completed the [Setup Guide](setup_guide.md) first.

## Quick Overview

This project uses **Bayesian Optimization** to find maximum values of unknown functions efficiently. The key idea:

1. Start with some initial data points
2. Build a probabilistic model (Gaussian Process)
3. Use the model to decide where to sample next
4. Repeat until you find the maximum

## 5-Minute Tutorial

### Step 1: Import Required Libraries

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.gaussian_process import GaussianProcessRegressor
from sklearn.gaussian_process.kernels import Matern
from scipy.stats import norm
```

### Step 2: Load Data

```python
# Load initial data for function 1
X_init = np.load("initial_data/function_1/initial_inputs.npy")
y_init = np.load("initial_data/function_1/initial_outputs.npy")

print(f"Initial data shape: X={X_init.shape}, y={y_init.shape}")
print(f"Input range: [{X_init.min():.3f}, {X_init.max():.3f}]")
print(f"Output range: [{y_init.min():.3e}, {y_init.max():.3e}]")
```

### Step 3: Initialize Gaussian Process

```python
# Define kernel
kernel = Matern(nu=2.5, length_scale=1.0)

# Create Gaussian Process model
gp = GaussianProcessRegressor(
    kernel=kernel,
    n_restarts_optimizer=10,
    alpha=1e-6,
    normalize_y=True,
    random_state=42
)

# Fit to initial data
gp.fit(X_init, y_init)

print("GP model trained successfully!")
```

### Step 4: Make Predictions

```python
# Create test points
X_test = np.random.uniform(0, 1, size=(100, 2))

# Predict with uncertainty
y_pred, y_std = gp.predict(X_test, return_std=True)

print(f"Predicted mean: {y_pred.mean():.6f}")
print(f"Prediction uncertainty: {y_std.mean():.6f}")
```

### Step 5: Calculate Acquisition Function

```python
def expected_improvement(X, gp, y_best, xi=0.01):
    """Calculate Expected Improvement acquisition function."""
    mu, sigma = gp.predict(X, return_std=True)
    
    # Calculate improvement
    imp = mu - y_best - xi
    Z = imp / sigma
    
    # Expected Improvement formula
    ei = imp * norm.cdf(Z) + sigma * norm.pdf(Z)
    ei[sigma == 0.0] = 0.0
    
    return ei

# Calculate EI for test points
y_best = y_init.max()
ei_values = expected_improvement(X_test, gp, y_best)

print(f"EI range: [{ei_values.min():.6e}, {ei_values.max():.6e}]")
```

### Step 6: Find Next Best Point

```python
# Find point with maximum Expected Improvement
best_idx = np.argmax(ei_values)
next_point = X_test[best_idx]

print(f"Next point to sample: {next_point}")
print(f"Expected Improvement: {ei_values[best_idx]:.6e}")
```

## Using the BayesianOptimizer Class

For a complete implementation, use the provided `BayesianOptimizer` class:

```python
# Initialize optimizer
optimizer = BayesianOptimizer(function_id=1, seed=42)

# Get next suggestion
next_point = optimizer.suggest_next_point(acquisition_func='ei')

# Format for submission
query = optimizer.format_query(next_point)
print(f"Next query: {query}")

# Get prediction at this point
mean, std = optimizer.predict_at_point(next_point)
print(f"Predicted value: {mean:.6f} ± {std:.6f}")
```

## Visualization Example

```python
import matplotlib.pyplot as plt

# For 2D functions
if X_init.shape[1] == 2:
    fig, axes = plt.subplots(1, 2, figsize=(12, 5))
    
    # Plot 1: Initial data points
    axes[0].scatter(X_init[:, 0], X_init[:, 1], c=y_init, cmap='viridis', s=100)
    axes[0].set_xlabel('x1')
    axes[0].set_ylabel('x2')
    axes[0].set_title('Initial Data Points')
    axes[0].colorbar()
    
    # Plot 2: Next suggested point
    axes[1].scatter(X_init[:, 0], X_init[:, 1], c=y_init, cmap='viridis', s=100, alpha=0.5)
    axes[1].scatter(next_point[0], next_point[1], c='red', s=200, marker='*', 
                   edgecolors='black', linewidth=2, label='Next Point')
    axes[1].set_xlabel('x1')
    axes[1].set_ylabel('x2')
    axes[1].set_title('Suggested Next Point')
    axes[1].legend()
    
    plt.tight_layout()
    plt.show()
```

## Common Use Cases

### 1. Single Function Optimization

```python
# Optimize function 1
optimizer = BayesianOptimizer(function_id=1, seed=42)

# Run 10 iterations
for i in range(10):
    next_point = optimizer.suggest_next_point()
    print(f"Iteration {i+1}: {optimizer.format_query(next_point)}")
```

### 2. Compare Acquisition Functions

```python
# Expected Improvement
ei_point = optimizer.suggest_next_point(acquisition_func='ei')
print(f"EI suggests: {ei_point}")

# Upper Confidence Bound
ucb_point = optimizer.suggest_next_point(acquisition_func='ucb')
print(f"UCB suggests: {ucb_point}")
```

### 3. Optimize All Functions

```python
results = {}

for func_id in range(1, 9):
    optimizer = BayesianOptimizer(function_id=func_id, seed=42)
    next_point = optimizer.suggest_next_point()
    results[func_id] = {
        'point': next_point,
        'query': optimizer.format_query(next_point)
    }
    print(f"Function {func_id}: {results[func_id]['query']}")
```

## Understanding the Results

### Interpretation Guide

1. **High Expected Improvement**: This point is likely to improve over current best
2. **High Uncertainty (std)**: The model is uncertain about this region (exploration)
3. **High Mean Prediction**: The model predicts a high value here (exploitation)
4. **Best Value Found**: The maximum observed value so far

### Key Metrics

```python
# After running optimization
print(f"Best value found: {optimizer.y_scaled.max():.6f}")
print(f"Number of evaluations: {len(optimizer.y_scaled)}")
print(f"GP kernel parameters: {optimizer.gp.kernel_}")
```

## Next Steps

### Learn More

1. **Methodology**: Read [methodology.md](methodology.md) for theoretical details
2. **Full Notebook**: Explore `Bayesian_optimisation-capsotne.ipynb` for complete implementation
3. **Documentation**: Check [DATASHEET.md](../DATASHEET.md) and [MODEL_CARD.md](../MODEL_CARD.md)

### Experiment

Try modifying:
- Acquisition function parameters (`xi`, `kappa`)
- Number of random candidates
- GP kernel parameters
- Different acquisition function strategies

### Advanced Topics

- Implement batch Bayesian Optimization
- Add constraints to the optimization
- Try different kernels (RBF, Rational Quadratic)
- Implement multi-objective optimization
- Apply to real-world hyperparameter tuning

## Troubleshooting

### Issue: GP training fails

```python
# Increase noise parameter
gp = GaussianProcessRegressor(alpha=1e-5)  # Instead of 1e-6
```

### Issue: Acquisition optimization suggests same point

```python
# Increase exploration
ei = expected_improvement(X, gp, y_best, xi=0.1)  # Instead of 0.01
```

### Issue: Poor convergence

```python
# Use UCB instead of EI
next_point = optimizer.suggest_next_point(acquisition_func='ucb')
```

## Resources

- [Scikit-learn GP Tutorial](https://scikit-learn.org/stable/modules/gaussian_process.html)
- [Distill Pub: Bayesian Optimization](https://distill.pub/2020/bayesian-optimization/)
- [Bayesian Optimization Book](http://www.gaussianprocess.org/gpml/)

## Questions?

- Check the [main README](../README.md)
- Browse [GitHub Issues](https://github.com/YOUR_USERNAME/bayesian-optimization-capstone/issues)
- See [CONTRIBUTING.md](../CONTRIBUTING.md) for how to get help

---

**Happy Optimizing! 🚀**

