# Model Card: Bayesian Optimization with Gaussian Process Regression

This model card follows the framework proposed by Mitchell et al. (2019) in ["Model Cards for Model Reporting"](https://arxiv.org/abs/1810.03993).

---

## Model Details

### Basic Information

- **Model Name**: Bayesian Optimizer with Gaussian Process Surrogate
- **Model Version**: 1.0
- **Model Type**: Bayesian Optimization Framework
- **Model Date**: October 2025
- **Model Developers**: Capstone Project Author
- **License**: MIT License
- **Contact**: See repository README

### Model Architecture

The model consists of three main components:

1. **Surrogate Model**: Gaussian Process Regressor
   - Kernel: Matérn (ν=2.5) 
   - Optimization: Maximum Likelihood Estimation of hyperparameters
   - Library: scikit-learn 

2. **Acquisition Functions**:
   - Expected Improvement (EI): Default for most problems
   - Upper Confidence Bound (UCB): Used for high-variance scenarios
   - Configurable exploration parameters (xi, kappa)

3. **Optimization Strategy**:
   - Sequential point selection
   - Adaptive acquisition function selection
   - Multi-start optimization for acquisition maximization

### Input/Output Specifications

**Input**:
- Dimension: Varies (2D to 8D depending on function)
- Format: NumPy array, shape (n_dimensions,)
- Range: [0, 1] for each dimension (normalized)

**Output**:
- Suggested next evaluation point
- Predicted function value with uncertainty
- Formatted query string for submission

---

## Intended Use

### Primary Intended Uses

1. **Function Optimization**: Finding maximum values of expensive-to-evaluate black-box functions
2. **Research and Education**: Teaching Bayesian Optimization concepts
3. **Hyperparameter Tuning**: Optimizing machine learning model hyperparameters
4. **Experimental Design**: Sequentially selecting experiments to maximize information gain

### Intended Users

- Machine learning researchers and practitioners
- Optimization specialists
- Data scientists working with expensive function evaluations
- Students learning about Bayesian methods and active learning

### Out-of-Scope Uses

The model should NOT be used for:

1. **Safety-Critical Applications**: Without extensive validation and safety guarantees
2. **Discrete Optimization**: The model assumes continuous input spaces
3. **Constrained Optimization**: Without modification to handle constraints
4. **Multi-Objective Optimization**: Currently designed for single-objective problems
5. **Real-Time Applications**: Optimization can be computationally expensive
6. **Very High Dimensions**: Performance degrades significantly beyond 20-30 dimensions

---

## Factors

### Relevant Factors

**Problem Characteristics**:
- **Dimensionality**: 2-8 dimensions tested; performance varies with dimension
- **Function Smoothness**: Assumes relatively smooth functions (suitable for GP modeling)
- **Noise Level**: Can handle some noise but performance degrades with high noise
- **Multi-modality**: May struggle with highly multi-modal functions

**Computational Factors**:
- **Evaluation Budget**: Designed for limited evaluation budgets (10-100 evaluations)
- **Computational Resources**: GP training time scales O(n³) with number of samples
- **Random Seed**: Results depend on initialization; reproducibility requires seed control

### Evaluation Factors

**Test Functions**:
- 8 functions with varying characteristics
- Dimensions: 2, 2, 3, 4, 4, 5, 6, 8
- Different ranges, scales, and complexities

---

## Metrics

### Model Performance Metrics

1. **Best Value Found**: Maximum function value discovered during optimization
2. **Simple Regret**: Difference between global optimum and best found value
3. **Convergence Rate**: How quickly the algorithm finds good solutions
4. **Sample Efficiency**: Quality of solution relative to number of evaluations

### Decision Thresholds

**Acquisition Function Parameters**:
- **EI xi (exploration)**: 0.01 (default), lower values emphasize exploitation
- **UCB kappa**: 2.576 (default), controls exploration-exploitation balance
- **Number of candidates**: 10,000 (for acquisition optimization)

### Variation in Performance

Performance varies across:
- **Dimensionality**: Lower dimensions generally easier to optimize
- **Function Landscape**: Smooth, unimodal functions easier than rugged, multi-modal ones
- **Initial Data Quality**: Better initial coverage leads to better optimization
- **Acquisition Function Choice**: EI vs UCB performs differently on different problems

---

## Evaluation Data

### Datasets

**Training**: Initial samples from each function (10-40 samples per function)

**Validation**: New evaluations acquired during optimization

**Test**: Final performance evaluated on the true function optimum (where known)

### Preprocessing

- Input normalization to [0, 1] range
- Standardization of outputs using MinMaxScaler
- No data augmentation

---

## Training Data

See [DATASHEET.md](DATASHEET.md) for complete training data documentation.

**Summary**:
- 8 functions with varying dimensionality
- 175 total initial samples
- Randomly sampled from uniform distribution
- Normalized input space [0, 1]^d

---

## Quantitative Analyses

### Performance by Dimensionality

| Dimension | Functions | Avg. Initial Samples | Complexity |
|-----------|-----------|---------------------|------------|
| 2D        | 1, 2      | 10                  | Low        |
| 3D        | 3         | 15                  | Medium     |
| 4D        | 4, 5      | 25                  | Medium     |
| 5D        | 6         | 20                  | High       |
| 6D        | 7         | 30                  | High       |
| 8D        | 8         | 40                  | Very High  |

### Acquisition Function Performance

- **Expected Improvement**: Better for exploitation, smoother convergence
- **Upper Confidence Bound**: Better for exploration, handles uncertainty well
- **Adaptive Selection**: Combining strategies improves robustness

### Computational Performance

- **GP Training Time**: ~0.01-0.5s per iteration (depending on sample size)
- **Acquisition Optimization**: ~0.1-1.0s per iteration
- **Scalability**: Feasible for up to 200-300 evaluations

---

## Ethical Considerations

### Potential Biases

1. **Initialization Bias**: Initial samples may not represent the entire function space
2. **Seed Dependence**: Random seed affects exploration patterns
3. **Acquisition Function Bias**: Choice of acquisition function affects which regions are explored

### Use Cases with Ethical Implications

If applied to real-world problems, consider:

1. **Fairness**: Optimization might favor certain regions/populations over others
2. **Transparency**: Black-box nature of optimization process
3. **Accountability**: Who is responsible for optimization decisions?
4. **Privacy**: If used with sensitive data, ensure proper data protection
5. **Environmental Impact**: Computational cost of GP training

### Mitigations

- Document all hyperparameters and random seeds for reproducibility
- Test multiple random initializations
- Compare different acquisition functions
- Validate results on held-out test data
- Provide uncertainty estimates with predictions

---

## Caveats and Recommendations

### Known Limitations

1. **Gaussian Assumption**: GP assumes function can be modeled with Gaussian distributions
2. **Stationarity**: Assumes function properties are consistent across the space
3. **Smoothness**: Struggles with discontinuous or highly irregular functions
4. **Computational Cost**: O(n³) scaling limits sample size
5. **High Dimensions**: Curse of dimensionality affects performance beyond 20-30D
6. **Local Optima**: May converge to local optima in multi-modal landscapes

### Recommendations

**For Users**:
1. Start with Expected Improvement for smoother functions
2. Use UCB for functions with high uncertainty or multi-modality
3. Ensure sufficient initial samples (at least 2-5x the dimensionality)
4. Normalize inputs to [0, 1] range
5. Use multiple random seeds and compare results
6. Monitor convergence and adjust acquisition parameters if needed

**For Developers**:
1. Consider sparse GP approximations for large datasets
2. Implement constraint handling for real-world problems
3. Add multi-objective optimization capabilities
4. Explore alternative kernels for non-smooth functions
5. Implement parallel/batch evaluation strategies

### When to Use Alternatives

Consider alternative methods when:
- **High Dimensions**: Use Random Search, CMA-ES, or gradient-based methods
- **Discrete Variables**: Use Tree-Parzen Estimator or genetic algorithms
- **Cheap Evaluations**: Use gradient-based optimization or random search
- **Non-Smooth Functions**: Use evolutionary algorithms or derivative-free methods
- **Multi-Objective**: Use NSGA-II, MOEA/D, or Bayesian multi-objective optimization

---

## Additional Information

### Model Assumptions

1. Function is continuous and relatively smooth
2. Function can be approximated by GP with chosen kernel
3. Evaluations are noise-free or have low noise
4. Input space can be normalized to [0, 1]^d
5. Sequential evaluation is acceptable (not batch-parallel)

### Software Dependencies

```
numpy >= 1.21.0
matplotlib >= 3.4.0
scikit-learn >= 1.0.0
scipy >= 1.7.0
```

### Related Work

**Foundational Papers**:
- Mockus, J. (1975). "On Bayesian Methods for Seeking the Extremum"
- Jones, D. R., et al. (1998). "Efficient Global Optimization of Expensive Black-Box Functions"
- Snoek, J., et al. (2012). "Practical Bayesian Optimization of Machine Learning Algorithms"

**Software Libraries**:
- GPyOpt
- Spearmint  
- Hyperopt
- Optuna
- Ax/BoTorch

### Citation

If you use this model in your research, please cite:

```bibtex
@misc{bayesian_opt_capstone_2025,
  author = {Your Name},
  title = {Bayesian Optimization for Function Maximization},
  year = {2025},
  publisher = {GitHub},
  url = {https://github.com/YOUR_USERNAME/bayesian-optimization-capstone}
}
```

---

## Model Card Changelog

### Version 1.0 (October 2025)
- Initial release
- Documented base Bayesian Optimizer with GP surrogate
- EI and UCB acquisition functions
- Tested on 8 benchmark functions

---

**Last Updated**: October 8, 2025  
**Model Card Version**: 1.0  
**Contact**: See repository README for contact information

---

## Feedback and Questions

For questions, issues, or suggestions regarding this model card or the model itself:
- Open an issue on the GitHub repository
- Contact the repository maintainer
- Submit a pull request with improvements

