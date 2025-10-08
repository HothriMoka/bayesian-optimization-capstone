# GitHub Repository Setup Guide

This guide walks you through setting up and pushing your project to GitHub.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Creating a GitHub Repository](#creating-a-github-repository)
3. [Initializing Git Locally](#initializing-git-locally)
4. [Pushing to GitHub](#pushing-to-github)
5. [Best Practices](#best-practices)
6. [Troubleshooting](#troubleshooting)

---

## Prerequisites

### 1. GitHub Account

If you don't have a GitHub account:
1. Go to [github.com](https://github.com)
2. Click "Sign up"
3. Follow the registration process

### 2. Git Installation

Check if Git is installed:
```bash
git --version
```

If not installed, download from [git-scm.com](https://git-scm.com/downloads)

### 3. Configure Git

Set your name and email (used for commits):
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Verify configuration:
```bash
git config --global --list
```

---

## Creating a GitHub Repository

### Step 1: Create New Repository

1. Go to [github.com](https://github.com) and log in
2. Click the "+" icon in top-right corner
3. Select "New repository"

### Step 2: Repository Settings

**Repository Name**: `bayesian-optimization-capstone`
(or your preferred name)

**Description**: 
```
Bayesian Optimization with Gaussian Processes for function maximization - Machine Learning Capstone Project
```

**Visibility**: 
- ✅ **Public** (recommended for portfolios)
- ⬜ Private (if you prefer)

**Initialize repository**: 
- ⬜ **DO NOT** check "Add a README file"
- ⬜ **DO NOT** add .gitignore
- ⬜ **DO NOT** choose a license

(We'll add these from the local repository)

### Step 3: Create Repository

Click "Create repository" button

**Save the repository URL**: You'll need it shortly
```
https://github.com/YOUR_USERNAME/bayesian-optimization-capstone.git
```

---

## Initializing Git Locally

### Step 1: Navigate to Project Directory

```bash
cd /Users/hmoka2/Desktop/Work/GitHub_repos/Machine-Learning/Capstone_Project
```

### Step 2: Initialize Git Repository

```bash
git init
```

You should see: `Initialized empty Git repository in ...`

### Step 3: Add Files to Staging

```bash
# Add all files (respects .gitignore)
git add .

# Check what will be committed
git status
```

### Step 4: Create Initial Commit

```bash
git commit -m "Initial commit: Complete Bayesian Optimization capstone project

- Add main Jupyter notebook with Bayesian Optimization implementation
- Add comprehensive README with non-technical summary
- Add DATASHEET.md for dataset documentation
- Add MODEL_CARD.md for model documentation
- Add requirements.txt with all dependencies
- Add initial data for 8 optimization functions
- Add complete documentation in docs/ directory
- Add LICENSE (MIT)
- Add CONTRIBUTING.md guidelines"
```

---

## Pushing to GitHub

### Step 1: Add Remote Repository

Replace `YOUR_USERNAME` with your actual GitHub username:

```bash
git remote add origin https://github.com/YOUR_USERNAME/bayesian-optimization-capstone.git
```

Verify remote was added:
```bash
git remote -v
```

### Step 2: Rename Branch to `main` (if needed)

```bash
# Check current branch name
git branch

# If it's not 'main', rename it
git branch -M main
```

### Step 3: Push to GitHub

```bash
git push -u origin main
```

**If prompted for credentials**:
- Username: Your GitHub username
- Password: Use a Personal Access Token (not your GitHub password)

### Creating a Personal Access Token (if needed)

1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Click "Generate new token (classic)"
3. Give it a descriptive name: "Capstone Project Upload"
4. Select scopes: ✅ `repo` (full control of private repositories)
5. Click "Generate token"
6. **Copy the token immediately** (you won't see it again)
7. Use this token as your password when pushing

---

## Verify Upload

1. Go to your GitHub repository URL
2. You should see all your files
3. Click on `README.md` to verify it displays correctly
4. Check that the Jupyter notebook is visible

---

## Best Practices

### 1. Good Commit Messages

**Format**:
```
<type>: <short description>

<detailed description (optional)>
```

**Types**:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `refactor`: Code refactoring
- `test`: Adding tests
- `chore`: Maintenance tasks

**Examples**:
```bash
git commit -m "docs: Update README with installation instructions"
git commit -m "feat: Add UCB acquisition function"
git commit -m "fix: Correct GP kernel parameter bounds"
```

### 2. Regular Commits

Don't wait to commit everything at once:
```bash
# After each logical change
git add <changed-files>
git commit -m "descriptive message"
git push
```

### 3. Check Status Often

```bash
git status  # See what's changed
git diff    # See detailed changes
git log     # See commit history
```

### 4. Use .gitignore

The `.gitignore` file is already set up to exclude:
- Python cache files (`__pycache__`)
- Jupyter checkpoints (`.ipynb_checkpoints`)
- Virtual environments (`venv/`)
- Large data files (optionally)

### 5. Protect Main Branch

On GitHub:
1. Go to Settings → Branches
2. Add rule for `main` branch
3. Enable "Require pull request reviews before merging"
4. Enable "Require status checks to pass"

---

## Additional Git Commands

### Updating Your Repository

After making changes:
```bash
git add .
git commit -m "Description of changes"
git push
```

### Pulling Latest Changes

If collaborating or working from multiple machines:
```bash
git pull origin main
```

### Creating Branches

For new features:
```bash
# Create and switch to new branch
git checkout -b feature/new-acquisition-function

# Make changes, commit
git add .
git commit -m "Add new acquisition function"

# Push branch to GitHub
git push -u origin feature/new-acquisition-function

# On GitHub, create a Pull Request to merge into main
```

### Viewing History

```bash
git log --oneline --graph --all
```

### Undoing Changes

```bash
# Discard changes in working directory
git checkout -- <file>

# Unstage file
git reset HEAD <file>

# Amend last commit
git commit --amend
```

---

## Repository Customization

### 1. Add Topics

On GitHub repository page:
1. Click "About" settings (gear icon)
2. Add topics: `machine-learning`, `bayesian-optimization`, `gaussian-processes`, `python`, `optimization`, `data-science`
3. Save changes

### 2. Add Description

In "About" settings:
- **Description**: "Bayesian Optimization with Gaussian Processes for function maximization"
- **Website**: (your portfolio/LinkedIn if applicable)

### 3. Enable GitHub Pages (Optional)

To host documentation:
1. Settings → Pages
2. Source: Deploy from a branch
3. Branch: `main`, folder: `/docs`
4. Save

### 4. Add Repository Banner

Create a banner image showcasing your project:
- Size: 1280 x 640 pixels
- Upload to repository as `banner.png`
- Reference in README

---

## Making Your Repository Stand Out

### 1. Complete README

✅ Non-technical summary (100 words)  
✅ Project overview  
✅ Installation instructions  
✅ Usage examples  
✅ Results and visualizations  
✅ Documentation links  

### 2. Add Badges

At the top of README.md:
```markdown
![Python Version](https://img.shields.io/badge/python-3.8%2B-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-complete-success)
```

### 3. Include Visualizations

Add plots and figures to make the README visually appealing:
- Optimization progress plots
- Acquisition function visualizations
- Results comparisons

### 4. Professional Documentation

✅ DATASHEET.md  
✅ MODEL_CARD.md  
✅ CONTRIBUTING.md  
✅ LICENSE  
✅ Comprehensive docs/ folder  

---

## Submission Checklist

Before submitting your GitHub link:

- [ ] All files are committed and pushed
- [ ] README.md displays correctly on GitHub
- [ ] Jupyter notebook renders properly on GitHub
- [ ] All links in README work
- [ ] DATASHEET.md is complete
- [ ] MODEL_CARD.md is complete
- [ ] requirements.txt includes all dependencies
- [ ] .gitignore excludes unnecessary files
- [ ] LICENSE file is present
- [ ] Repository is public (if required)
- [ ] Repository description is set
- [ ] Topics/tags are added
- [ ] All documentation files are present
- [ ] Initial data files are included

---

## Troubleshooting

### Issue: "Permission denied (publickey)"

**Solution**: Use HTTPS instead of SSH or set up SSH keys

```bash
# Check current remote URL
git remote -v

# If using SSH, switch to HTTPS
git remote set-url origin https://github.com/YOUR_USERNAME/bayesian-optimization-capstone.git
```

### Issue: "Updates were rejected"

**Solution**: Pull changes first

```bash
git pull origin main --rebase
git push origin main
```

### Issue: Large files rejected

**Solution**: Use Git LFS or link to external storage

```bash
# Install Git LFS
git lfs install

# Track large files
git lfs track "*.csv"
git lfs track "*.npy"

# Commit and push
git add .gitattributes
git commit -m "Configure Git LFS"
git push
```

**Note**: `.npy` files in this project are small enough for regular Git.

### Issue: Merge conflicts

**Solution**: Resolve conflicts manually

```bash
# Open conflicted files and resolve
# Look for <<<<<<, ======, >>>>>> markers

# After resolving
git add <resolved-files>
git commit -m "Resolve merge conflicts"
git push
```

---

## Additional Resources

- [GitHub Docs](https://docs.github.com)
- [Git Handbook](https://guides.github.com/introduction/git-handbook/)
- [Pro Git Book](https://git-scm.com/book/en/v2) (free)
- [GitHub Skills](https://skills.github.com/) (interactive tutorials)
- [Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)

---

## Getting Help

If you encounter issues:

1. Check GitHub's [Status Page](https://www.githubstatus.com/)
2. Search [Stack Overflow](https://stackoverflow.com/questions/tagged/git)
3. Ask in GitHub [Community Forum](https://github.community/)
4. Consult [Git Documentation](https://git-scm.com/doc)

---

## Next Steps

After pushing to GitHub:

1. ✅ Verify all files are visible
2. ✅ Test that README displays correctly
3. ✅ Share repository link for submission
4. ✅ Add to your portfolio/resume
5. ✅ Share on LinkedIn

**Your GitHub URL for submission**:
```
https://github.com/YOUR_USERNAME/bayesian-optimization-capstone
```

---

**Congratulations! Your project is now on GitHub! 🎉**

