# Data Guide

This guide explains the datasets used in this project and how to work with them.

## Table of Contents

1. [Initial Function Data](#initial-function-data)
2. [Gene Expression Dataset (Reference)](#gene-expression-dataset-reference)
3. [Data Loading](#data-loading)
4. [Data Format](#data-format)
5. [Data Characteristics](#data-characteristics)

---

## Initial Function Data

### Overview

The project includes initial training data for 8 optimization functions stored in the `initial_data/` directory.

### Directory Structure

```
initial_data/
├── function_1/
│   ├── initial_inputs.npy    # 2D inputs (10 samples)
│   └── initial_outputs.npy   # Function outputs (10 values)
├── function_2/
│   ├── initial_inputs.npy    # 2D inputs (10 samples)
│   └── initial_outputs.npy
├── function_3/
│   ├── initial_inputs.npy    # 3D inputs (15 samples)
│   └── initial_outputs.npy
├── function_4/
│   ├── initial_inputs.npy    # 4D inputs (30 samples)
│   └── initial_outputs.npy
├── function_5/
│   ├── initial_inputs.npy    # 4D inputs (20 samples)
│   └── initial_outputs.npy
├── function_6/
│   ├── initial_inputs.npy    # 5D inputs (20 samples)
│   └── initial_outputs.npy
├── function_7/
│   ├── initial_inputs.npy    # 6D inputs (30 samples)
│   └── initial_outputs.npy
└── function_8/
    ├── initial_inputs.npy    # 8D inputs (40 samples)
    └── initial_outputs.npy
```

### Function Specifications

| Function | Dimensions | Samples | Input Range | Output Range (approx) |
|----------|-----------|---------|-------------|---------------------|
| 1        | 2         | 10      | [0, 1]      | [0, 1e-79]          |
| 2        | 2         | 10      | [0, 1]      | [-0.07, 0.61]       |
| 3        | 3         | 15      | [0, 1]      | [-0.40, -0.04]      |
| 4        | 4         | 30      | [0, 1]      | [-33, -4]           |
| 5        | 4         | 20      | [0, 1]      | [0.11, 1089]        |
| 6        | 5         | 20      | [0, 1]      | [-2.57, -0.71]      |
| 7        | 6         | 30      | [0, 1]      | [0, 1.37]           |
| 8        | 8         | 40      | [0, 1]      | [5.59, 9.60]        |

---

## Gene Expression Dataset (Reference)

### About the Dataset

The Gene Expression Bioinformatics Dataset is referenced as a real-world application example of optimization techniques similar to those used in this project.

**Source**: [Kaggle - Gene Expression Bioinformatics Dataset](https://www.kaggle.com/datasets/samira1992/gene-expression-bioinformatics-dataset)

### Dataset Description

- **Purpose**: Gene expression analysis and disease classification
- **Application**: Demonstrates where Bayesian Optimization is valuable for hyperparameter tuning in bioinformatics
- **Note**: This dataset is **not directly used** in the optimization tasks but provides context for real-world applications

### Use Cases in Bioinformatics

1. **Hyperparameter Tuning**: Optimize ML model parameters for gene classification
2. **Feature Selection**: Find optimal subset of genes for prediction
3. **Experimental Design**: Select which genes to sequence next
4. **Biomarker Discovery**: Identify important genetic markers

### How to Download (Optional)

If you want to explore the gene expression dataset:

1. Create a Kaggle account at [kaggle.com](https://www.kaggle.com)
2. Visit the [dataset page](https://www.kaggle.com/datasets/samira1992/gene-expression-bioinformatics-dataset)
3. Click "Download" button
4. Extract to a separate directory (not required for this project)

**Note**: This is optional and not needed to run the main optimization tasks.

---

## Data Loading

### Loading Initial Function Data

#### Basic Loading

```python
import numpy as np

# Load data for function 1
function_id = 1
X = np.load(f"initial_data/function_{function_id}/initial_inputs.npy")
y = np.load(f"initial_data/function_{function_id}/initial_outputs.npy")

print(f"Inputs shape: {X.shape}")
print(f"Outputs shape: {y.shape}")
```

#### Loading All Functions

```python
def load_function_data(function_id):
    """Load initial data for a specific function."""
    X = np.load(f"initial_data/function_{function_id}/initial_inputs.npy")
    y = np.load(f"initial_data/function_{function_id}/initial_outputs.npy")
    return X, y

# Load all functions
all_data = {}
for i in range(1, 9):
    X, y = load_function_data(i)
    all_data[i] = {'X': X, 'y': y}
    print(f"Function {i}: X{X.shape}, y{y.shape}")
```

### Data Inspection

```python
# Examine function 1
X, y = load_function_data(1)

print("=" * 50)
print(f"Function 1 Analysis")
print("=" * 50)
print(f"Input dimensionality: {X.shape[1]}")
print(f"Number of samples: {X.shape[0]}")
print(f"\nInput statistics:")
print(f"  Min: {X.min(axis=0)}")
print(f"  Max: {X.max(axis=0)}")
print(f"  Mean: {X.mean(axis=0)}")
print(f"\nOutput statistics:")
print(f"  Min: {y.min():.6e}")
print(f"  Max: {y.max():.6e}")
print(f"  Mean: {y.mean():.6e}")
print(f"  Std: {y.std():.6e}")
```

---

## Data Format

### Input Files (`initial_inputs.npy`)

**Format**: NumPy binary array (`.npy`)

**Structure**:
- Shape: `(n_samples, n_dimensions)`
- Data type: `float64`
- Range: [0, 1] for each dimension

**Example**:
```python
# Function 1 inputs (2D)
X = np.array([
    [0.319, 0.763],  # Sample 1
    [0.574, 0.880],  # Sample 2
    [0.731, 0.733],  # Sample 3
    # ...
])
```

### Output Files (`initial_outputs.npy`)

**Format**: NumPy binary array (`.npy`)

**Structure**:
- Shape: `(n_samples,)`
- Data type: `float64`
- Range: Function-specific

**Example**:
```python
# Function 1 outputs
y = np.array([1.32e-79, 1.03e-46, 7.71e-16, ...])
```

### Saving New Data

```python
# After getting new evaluations
X_new = np.array([[0.5, 0.5]])  # New input point
y_new = 0.123                    # New evaluation

# Append to existing data
X_updated = np.vstack([X, X_new])
y_updated = np.append(y, y_new)

# Save (optional - for your own experiments)
np.save("my_updated_inputs.npy", X_updated)
np.save("my_updated_outputs.npy", y_updated)
```

---

## Data Characteristics

### Input Space Properties

1. **Normalization**: All inputs are normalized to [0, 1]
2. **Sampling**: Uniformly random sampled from the input domain
3. **Coverage**: Attempts to cover the input space reasonably well
4. **Independence**: Each dimension is independently sampled

### Output Space Properties

1. **Scale Variation**: Different functions have vastly different output scales
2. **Non-negativity**: Some functions have only positive outputs
3. **Noise**: Minimal to no observation noise
4. **Smoothness**: Functions are generally smooth (suitable for GP modeling)

### Data Quality Checks

```python
def check_data_quality(X, y, function_id):
    """Perform quality checks on function data."""
    print(f"\n{'='*50}")
    print(f"Data Quality Check: Function {function_id}")
    print(f"{'='*50}")
    
    # Check shapes
    assert len(X.shape) == 2, "Inputs should be 2D array"
    assert len(y.shape) == 1, "Outputs should be 1D array"
    assert X.shape[0] == y.shape[0], "Number of inputs and outputs must match"
    print(f"✓ Shape consistency: {X.shape[0]} samples")
    
    # Check ranges
    assert X.min() >= 0 and X.max() <= 1, "Inputs should be in [0, 1]"
    print(f"✓ Input range: [{X.min():.3f}, {X.max():.3f}]")
    
    # Check for NaN/Inf
    assert not np.isnan(X).any(), "Inputs contain NaN"
    assert not np.isnan(y).any(), "Outputs contain NaN"
    assert not np.isinf(X).any(), "Inputs contain Inf"
    assert not np.isinf(y).any(), "Outputs contain Inf"
    print(f"✓ No NaN or Inf values")
    
    # Check for duplicates
    unique_rows = np.unique(X, axis=0).shape[0]
    if unique_rows < X.shape[0]:
        print(f"⚠ Warning: {X.shape[0] - unique_rows} duplicate input(s)")
    else:
        print(f"✓ No duplicate inputs")
    
    print(f"✓ All quality checks passed!")

# Run for all functions
for i in range(1, 9):
    X, y = load_function_data(i)
    check_data_quality(X, y, i)
```

### Data Statistics Summary

```python
import pandas as pd

# Create summary statistics
stats = []
for i in range(1, 9):
    X, y = load_function_data(i)
    stats.append({
        'Function': i,
        'Dimensions': X.shape[1],
        'Samples': X.shape[0],
        'Y Min': y.min(),
        'Y Max': y.max(),
        'Y Mean': y.mean(),
        'Y Std': y.std()
    })

df = pd.DataFrame(stats)
print(df.to_string(index=False))
```

---

## Working with Data

### Preprocessing

```python
from sklearn.preprocessing import MinMaxScaler

def preprocess_data(X, y):
    """Preprocess data for GP modeling."""
    # Inputs already normalized to [0, 1]
    
    # Scale outputs
    scaler = MinMaxScaler()
    y_scaled = scaler.fit_transform(y.reshape(-1, 1)).ravel()
    
    return X, y_scaled, scaler

# Example
X, y = load_function_data(1)
X_processed, y_processed, scaler = preprocess_data(X, y)
```

### Train-Test Split (For Validation)

```python
from sklearn.model_selection import train_test_split

# If you want to validate your GP
X, y = load_function_data(1)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

print(f"Training set: {X_train.shape}")
print(f"Test set: {X_test.shape}")
```

### Data Augmentation (Sequential)

```python
class DataBuffer:
    """Buffer to store and update data during optimization."""
    
    def __init__(self, X_init, y_init):
        self.X = X_init.copy()
        self.y = y_init.copy()
    
    def add_point(self, x_new, y_new):
        """Add new evaluation."""
        self.X = np.vstack([self.X, x_new.reshape(1, -1)])
        self.y = np.append(self.y, y_new)
    
    def get_data(self):
        """Return current data."""
        return self.X, self.y
    
    def get_best(self):
        """Return best observed value."""
        best_idx = np.argmax(self.y)
        return self.X[best_idx], self.y[best_idx]

# Usage
X, y = load_function_data(1)
buffer = DataBuffer(X, y)

# Simulate adding new points
buffer.add_point(np.array([0.5, 0.5]), 0.123)
X_updated, y_updated = buffer.get_data()
print(f"Updated data: {X_updated.shape}")
```

---

## Best Practices

1. **Always check data shapes** before feeding to models
2. **Verify input ranges** are in [0, 1]
3. **Handle different output scales** with appropriate normalization
4. **Save intermediate results** during long optimization runs
5. **Use version control** for any data modifications

---

## Data Citation

If you use this data in your research or publications, please cite:

```bibtex
@misc{bayesian_opt_data_2025,
  author = {Your Name},
  title = {Initial Data for Bayesian Optimization Functions},
  year = {2025},
  publisher = {GitHub},
  url = {https://github.com/YOUR_USERNAME/bayesian-optimization-capstone}
}
```

---

## Additional Resources

- [NumPy I/O Documentation](https://numpy.org/doc/stable/reference/routines.io.html)
- [Scikit-learn Preprocessing](https://scikit-learn.org/stable/modules/preprocessing.html)
- [DATASHEET.md](../DATASHEET.md) - Complete dataset documentation

---

**Need Help?** See the main [README](../README.md) or open an [issue](https://github.com/YOUR_USERNAME/bayesian-optimization-capstone/issues).

