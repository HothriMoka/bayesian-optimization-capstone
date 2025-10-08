# Setup Guide

This guide will help you set up the Bayesian Optimization project on your local machine.

## Prerequisites

Before you begin, ensure you have the following installed:

- **Python 3.8 or higher**: [Download Python](https://www.python.org/downloads/)
- **pip**: Package installer for Python (usually comes with Python)
- **Git**: Version control system [Download Git](https://git-scm.com/downloads)
- **Jupyter Notebook**: Will be installed via requirements.txt

### Check Your Python Version

```bash
python --version
# or
python3 --version
```

You should see Python 3.8.x or higher.

## Installation Steps

### 1. Clone the Repository

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/bayesian-optimization-capstone.git

# Navigate to the project directory
cd bayesian-optimization-capstone
```

### 2. Create a Virtual Environment (Recommended)

Using a virtual environment helps isolate project dependencies.

**On macOS/Linux:**
```bash
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate
```

**On Windows:**
```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
venv\Scripts\activate
```

You should see `(venv)` in your terminal prompt, indicating the virtual environment is active.

### 3. Install Dependencies

With the virtual environment activated:

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

This will install:
- NumPy
- SciPy
- Scikit-learn
- Matplotlib
- Seaborn
- Jupyter
- Pandas
- tqdm

**Installation time**: Approximately 2-5 minutes depending on your internet connection.

### 4. Verify Installation

Test that key packages are installed correctly:

```bash
python -c "import numpy; import sklearn; import matplotlib; print('All packages imported successfully!')"
```

If you see "All packages imported successfully!", you're good to go!

## Running the Project

### Launch Jupyter Notebook

```bash
jupyter notebook
```

This will open Jupyter in your default web browser.

### Open the Main Notebook

In the Jupyter interface:
1. Navigate to the project directory
2. Click on `Bayesian_optimisation-capsotne.ipynb`
3. Run the cells sequentially using `Shift + Enter`

### Expected Runtime

- **Setup cells**: < 1 second
- **Data loading**: < 1 second
- **Single optimization run**: 5-30 seconds (depending on function)
- **Complete notebook**: 5-15 minutes

## Project Structure

After setup, your directory should look like this:

```
bayesian-optimization-capstone/
├── README.md
├── LICENSE
├── DATASHEET.md
├── MODEL_CARD.md
├── CONTRIBUTING.md
├── requirements.txt
├── .gitignore
├── Bayesian_optimisation-capsotne.ipynb
├── initial_data/
│   ├── function_1/
│   │   ├── initial_inputs.npy
│   │   └── initial_outputs.npy
│   └── ... (functions 2-8)
└── docs/
    ├── methodology.md
    ├── setup_guide.md
    └── data_guide.md
```

## Troubleshooting

### Issue: `pip` not found

**Solution**: Make sure pip is installed:
```bash
python -m ensurepip --upgrade
```

### Issue: Permission denied when installing packages

**Solution**: Use virtual environment (recommended) or install with `--user` flag:
```bash
pip install --user -r requirements.txt
```

### Issue: ImportError for specific package

**Solution**: Install the specific package manually:
```bash
pip install package-name
```

### Issue: Jupyter not opening in browser

**Solution**: 
1. Check if Jupyter is running: You should see a URL in the terminal
2. Manually copy the URL (e.g., `http://localhost:8888/?token=...`) into your browser
3. Or specify browser:
   ```bash
   jupyter notebook --browser=chrome
   ```

### Issue: Notebook kernel not connecting

**Solution**:
```bash
# Install kernel for your virtual environment
python -m ipykernel install --user --name=venv
```

### Issue: Out of memory when running GP

**Solution**: 
- Close other applications
- Reduce `n_candidates` parameter in acquisition optimization
- Consider using sparse GP approximations for large datasets

## Updating the Project

To get the latest changes:

```bash
git pull origin main
pip install -r requirements.txt --upgrade
```

## Uninstallation

To remove the project and virtual environment:

```bash
# Deactivate virtual environment
deactivate

# Remove the project directory
cd ..
rm -rf bayesian-optimization-capstone
```

## Additional Resources

- [Python Virtual Environments Guide](https://docs.python.org/3/tutorial/venv.html)
- [Jupyter Notebook Documentation](https://jupyter-notebook.readthedocs.io/)
- [Scikit-learn Documentation](https://scikit-learn.org/stable/documentation.html)
- [NumPy Documentation](https://numpy.org/doc/)

## Getting Help

If you encounter issues not covered here:

1. Check [GitHub Issues](https://github.com/YOUR_USERNAME/bayesian-optimization-capstone/issues)
2. Search for similar problems in Stack Overflow
3. Create a new issue with:
   - Your Python version
   - Your operating system
   - Complete error message
   - Steps to reproduce

---

**Next Steps**: Once setup is complete, see the [Quick Start Guide](quick_start.md) to begin using the project!

