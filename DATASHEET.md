# Datasheet for Bayesian Optimization Function Dataset

This datasheet follows the framework proposed by Gebru et al. (2018) in ["Datasheets for Datasets"](https://arxiv.org/abs/1803.09010).

---

## Motivation

### 1. For what purpose was the dataset created?

The dataset was created to provide initial training data for Bayesian Optimization experiments on eight unknown mathematical functions with varying dimensionality (2D to 8D). The purpose is to bootstrap the optimization process with representative samples from each function's input-output space.

### 2. Who created the dataset and on behalf of which entity?

The dataset was created for a Machine Learning capstone project as part of an academic program focusing on advanced optimization techniques.

### 3. Who funded the creation of the dataset?

This dataset was created as part of an educational project. No external funding was involved.

---

## Composition

### 1. What do the instances that comprise the dataset represent?

Each instance represents a sampled input-output pair from an unknown mathematical function:
- **Input**: A point in the function's domain (normalized to [0, 1] in each dimension)
- **Output**: The function's evaluation at that input point

### 2. How many instances are there in total?

- **Function 1**: 10 samples (2D input space)
- **Function 2**: 10 samples (2D input space)
- **Function 3**: 15 samples (3D input space)
- **Function 4**: 30 samples (4D input space)
- **Function 5**: 20 samples (4D input space)
- **Function 6**: 20 samples (5D input space)
- **Function 7**: 30 samples (6D input space)
- **Function 8**: 40 samples (8D input space)

**Total**: 175 samples across all functions

### 3. Does the dataset contain all possible instances or is it a sample?

This is a sample. Each dataset contains initial samples from a continuous function domain. The complete instance space would be infinite (all points in the continuous input domain).

### 4. What data does each instance consist of?

Each instance consists of:
- **Input vector**: NumPy array of shape (n_dimensions,) with values in [0, 1]
- **Output scalar**: NumPy float representing the function evaluation

### 5. Is there a label or target associated with each instance?

Yes, the output value serves as the target. The task is to learn the mapping from inputs to outputs to maximize the function.

### 6. Is any information missing from individual instances?

No. Each instance has complete input-output pairs.

### 7. Are relationships between individual instances made explicit?

No explicit relationships are encoded. However, instances are implicitly related through their proximity in the input space and the smoothness of the underlying function.

### 8. Are there recommended data splits?

No splits are recommended. All initial data should be used for training the Gaussian Process surrogate model. New data is acquired sequentially through the optimization process.

### 9. Are there any errors, sources of noise, or redundancies in the dataset?

- **Errors**: No known errors in the data generation process
- **Noise**: The functions may include some observational noise, but this is typical in real-world optimization scenarios
- **Redundancies**: No duplicate samples exist in the initial datasets

### 10. Is the dataset self-contained?

Yes, the dataset is self-contained for its intended use in Bayesian Optimization.

---

## Collection Process

### 1. How was the data associated with each instance acquired?

The data was generated through evaluation of pre-defined mathematical functions at randomly sampled input points. The exact nature of these functions is unknown to the optimizer (black-box scenario).

### 2. What mechanisms or procedures were used to collect the data?

- Random sampling from uniform distribution over the normalized input space [0, 1]^d
- Function evaluation at sampled points
- Storage in NumPy array format

### 3. If the dataset is a sample from a larger set, what was the sampling strategy?

Random uniform sampling was used to provide diverse coverage of the input space while keeping the initial sample size manageable.

### 4. Who was involved in the data collection process?

The data was generated programmatically without human annotation.

### 5. Over what timeframe was the data collected?

The data was generated as part of the project initialization phase.

### 6. Were any ethical review processes conducted?

Not applicable - the dataset consists of mathematical function evaluations without any personal, sensitive, or human-subjects data.

---

## Preprocessing/Cleaning/Labeling

### 1. Was any preprocessing/cleaning/labeling of the data done?

**Preprocessing**:
- Input values normalized to [0, 1] range
- Data stored in NumPy binary format (.npy) for efficient loading

**Cleaning**: Not required - data generated programmatically

**Labeling**: Not applicable - supervised outputs are the function evaluations

### 2. Was the "raw" data saved in addition to the preprocessed/cleaned/labeled data?

The stored data represents the raw evaluations. No additional raw data exists.

### 3. Is the software used to preprocess/clean/label the instances available?

Yes, standard Python libraries (NumPy) were used, which are open-source and widely available.

---

## Uses

### 1. Has the dataset been used for any tasks already?

Yes, the dataset is used to initialize Bayesian Optimization runs for function maximization.

### 2. Is there a repository that links to any or all papers or systems that use the dataset?

This GitHub repository serves as the primary documentation and usage example.

### 3. What (other) tasks could the dataset be used for?

- Testing surrogate modeling techniques (Gaussian Processes, Random Forests, Neural Networks)
- Benchmarking global optimization algorithms
- Teaching and demonstrating active learning concepts
- Comparing different acquisition function strategies

### 4. Is there anything about the composition of the dataset or the way it was collected and preprocessed/cleaned/labeled that might impact future uses?

**Limitations**:
- Limited initial sample sizes may not fully represent complex function behaviors
- Normalized input space requires transformation for problems with different bounds
- Unknown function properties (smoothness, multi-modality) may affect generalization

### 5. Are there tasks for which the dataset should not be used?

The dataset should NOT be used for:
- Real-world decision-making without validation
- Applications requiring certified optimization guarantees
- Problems where the underlying function differs significantly from smooth, continuous functions

---

## Distribution

### 1. Will the dataset be distributed to third parties outside of the entity?

Yes, the dataset is publicly available through this GitHub repository.

### 2. How will the dataset be distributed?

- GitHub repository hosting
- Direct file download from repository
- Can be cloned with the entire project

### 3. When will the dataset be distributed?

The dataset is available immediately upon repository publication.

### 4. Will the dataset be distributed under a copyright or other intellectual property (IP) license?

Yes, distributed under the MIT License, allowing free use, modification, and distribution with attribution.

### 5. Have any third parties imposed IP-based or other restrictions on the data?

No restrictions apply.

### 6. Do any export controls or other regulatory restrictions apply to the dataset?

No export controls or regulatory restrictions apply.

---

## Maintenance

### 1. Who will be supporting/hosting/maintaining the dataset?

The repository owner maintains the dataset as part of the capstone project.

### 2. How can the owner/curator/manager of the dataset be contacted?

Through GitHub issues on the repository or direct contact information provided in the README.

### 3. Is there an erratum?

No erratum currently exists. Any discovered issues will be documented in the GitHub repository's issues section.

### 4. Will the dataset be updated?

The initial datasets are fixed for reproducibility. Any extensions or variants will be clearly versioned and documented.

### 5. If the dataset relates to people, are there applicable limits on the retention of the data?

Not applicable - the dataset does not relate to people.

### 6. Will older versions of the dataset continue to be supported/hosted/maintained?

Yes, through Git version control, all historical versions remain accessible.

### 7. If others want to extend/augment/build on/contribute to the dataset, is there a mechanism for them to do so?

Yes, contributions can be made through:
- GitHub pull requests
- Forking the repository
- Opening issues for suggestions

---

## Additional Information

### Dataset Format

Files are stored in NumPy binary format (.npy):
- `initial_inputs.npy`: Shape (n_samples, n_dimensions)
- `initial_outputs.npy`: Shape (n_samples,)

### Loading Example

```python
import numpy as np

# Load data for function 1
inputs = np.load("initial_data/function_1/initial_inputs.npy")
outputs = np.load("initial_data/function_1/initial_outputs.npy")

print(f"Inputs shape: {inputs.shape}")
print(f"Outputs shape: {outputs.shape}")
```

### Input Space Characteristics

| Function | Dimensions | Samples | Input Range | Output Range |
|----------|-----------|---------|-------------|--------------|
| 1        | 2         | 10      | [0, 1]      | ~[0, 0]      |
| 2        | 2         | 10      | [0, 1]      | ~[-0.07, 0.61] |
| 3        | 3         | 15      | [0, 1]      | ~[-0.40, -0.04] |
| 4        | 4         | 30      | [0, 1]      | ~[-33, -4]   |
| 5        | 4         | 20      | [0, 1]      | ~[0.11, 1089] |
| 6        | 5         | 20      | [0, 1]      | ~[-2.57, -0.71] |
| 7        | 6         | 30      | [0, 1]      | ~[0, 1.37]   |
| 8        | 8         | 40      | [0, 1]      | ~[5.59, 9.60] |

### Known Limitations

1. **Sample Size**: Limited initial samples may not capture all function characteristics
2. **Dimensionality**: Higher-dimensional functions (7, 8) are more challenging to optimize
3. **Noise**: Potential observational noise in function evaluations
4. **Generalization**: Performance on these functions may not generalize to all optimization problems

---

**Version**: 1.0  
**Date**: October 2025  
**Contact**: See repository README for contact information

