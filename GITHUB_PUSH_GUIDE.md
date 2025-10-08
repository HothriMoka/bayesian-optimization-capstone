# GitHub Push Guide - Step by Step

## Quick Steps to Push Your Project to GitHub

Follow these steps exactly to upload your capstone project to GitHub.

---

## Prerequisites Check

Before starting, verify you have:

```bash
# Check Git is installed
git --version

# Check Python is installed
python --version

# Check you're in the project directory
pwd
# Should show: /Users/hmoka2/Desktop/Work/GitHub_repos/Machine-Learning/Capstone_Project
```

---

## Step 1: Create GitHub Repository

### 1.1 Log in to GitHub

Go to [github.com](https://github.com) and log in.

### 1.2 Create New Repository

1. Click the **"+"** icon in top-right corner
2. Select **"New repository"**

### 1.3 Configure Repository

**Repository name**: `bayesian-optimization-capstone`

**Description**: 
```
Bayesian Optimization with Gaussian Processes for function maximization - Machine Learning Capstone Project
```

**Visibility**: 
- ✅ Select **Public**

**DO NOT** check any of these boxes:
- ⬜ Add a README file
- ⬜ Add .gitignore
- ⬜ Choose a license

### 1.4 Create Repository

Click **"Create repository"** button.

### 1.5 Copy Repository URL

You'll see a page with setup instructions. Copy the repository URL:
```
https://github.com/YOUR_USERNAME/bayesian-optimization-capstone.git
```

**Save this URL** - you'll need it in Step 3!

---

## Step 2: Initialize Local Git Repository

Open your terminal and navigate to the project directory:

```bash
cd /Users/hmoka2/Desktop/Work/GitHub_repos/Machine-Learning/Capstone_Project
```

### 2.1 Initialize Git

```bash
git init
```

**Expected output**: `Initialized empty Git repository in ...`

### 2.2 Configure Git (if not already done)

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Replace with your actual name and email.

### 2.3 Check Status

```bash
git status
```

You should see many untracked files listed in red.

---

## Step 3: Add and Commit Files

### 3.1 Add All Files

```bash
git add .
```

This stages all files (respecting .gitignore rules).

### 3.2 Verify What Will Be Committed

```bash
git status
```

You should see files in green. Files excluded by .gitignore won't appear.

**Expected to be excluded** (due to .gitignore):
- leaderboard-*.csv
- *.zip files
- Bayesian_optimisation_round3.ipynb
- 1023_data.csv

**Expected to be included**:
- README.md
- DATASHEET.md
- MODEL_CARD.md
- Bayesian_optimisation-capsotne.ipynb
- requirements.txt
- LICENSE
- All docs/
- All initial_data/

### 3.3 Create Initial Commit

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
- Add CONTRIBUTING.md and submission guides"
```

**Expected output**: Details about files committed.

---

## Step 4: Connect to GitHub

### 4.1 Add Remote Repository

Replace `YOUR_USERNAME` with your **actual GitHub username**:

```bash
git remote add origin https://github.com/YOUR_USERNAME/bayesian-optimization-capstone.git
```

### 4.2 Verify Remote

```bash
git remote -v
```

**Expected output**:
```
origin  https://github.com/YOUR_USERNAME/bayesian-optimization-capstone.git (fetch)
origin  https://github.com/YOUR_USERNAME/bayesian-optimization-capstone.git (push)
```

### 4.3 Rename Branch to Main

```bash
git branch -M main
```

This ensures your branch is called "main" (GitHub standard).

---

## Step 5: Push to GitHub

### 5.1 Push Your Code

```bash
git push -u origin main
```

### 5.2 Authentication

You'll be prompted for credentials:

**Username**: Your GitHub username

**Password**: **Use a Personal Access Token** (NOT your GitHub password)

#### Creating a Personal Access Token

If you don't have a token:

1. Go to GitHub: Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Click "Generate new token (classic)"
3. Name: `Capstone Project Upload`
4. Expiration: Choose your preference (e.g., 90 days)
5. Scopes: Check ✅ **repo** (full control of private repositories)
6. Click "Generate token"
7. **COPY THE TOKEN IMMEDIATELY** (you won't see it again!)
8. Use this token as your password when pushing

### 5.3 Wait for Upload

The upload may take 1-2 minutes. You'll see progress:
```
Enumerating objects: XX, done.
Counting objects: 100% (XX/XX), done.
...
```

**Expected final output**:
```
To https://github.com/YOUR_USERNAME/bayesian-optimization-capstone.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

---

## Step 6: Verify Upload

### 6.1 Visit Your Repository

Go to:
```
https://github.com/YOUR_USERNAME/bayesian-optimization-capstone
```

### 6.2 Checklist

Verify you can see:
- [ ] README.md displays as the homepage
- [ ] All documentation files (DATASHEET.md, MODEL_CARD.md, etc.)
- [ ] Jupyter notebook (Bayesian_optimisation-capsotne.ipynb)
- [ ] docs/ folder with all guides
- [ ] initial_data/ folder with function data
- [ ] requirements.txt
- [ ] LICENSE file

### 6.3 Test Notebook Rendering

1. Click on `Bayesian_optimisation-capsotne.ipynb`
2. Verify it renders correctly on GitHub
3. Check that code and outputs are visible

### 6.4 Test Links

1. Open README.md on GitHub
2. Click various links to ensure they work
3. Verify internal links navigate correctly

---

## Step 7: Customize Repository

### 7.1 Add Description and Topics

1. On your repository page, click the **gear icon** next to "About"
2. **Description**: Add the project description
3. **Topics**: Add tags like:
   - `machine-learning`
   - `bayesian-optimization`
   - `gaussian-processes`
   - `python`
   - `optimization`
   - `data-science`
   - `capstone-project`
4. Click "Save changes"

### 7.2 Verify Repository is Public

1. Go to Settings → General
2. Scroll to "Danger Zone"
3. Verify it says "Change repository visibility" with current status: **Public**

---

## Step 8: Final Verification

### 8.1 Test Clone

In a different directory, test cloning:

```bash
cd /tmp
git clone https://github.com/YOUR_USERNAME/bayesian-optimization-capstone.git
cd bayesian-optimization-capstone
ls -la
```

Verify all files are present.

### 8.2 Test Setup

```bash
pip install -r requirements.txt
```

Verify dependencies install correctly.

---

## Step 9: Get Submission URL

Your submission URL is:

```
https://github.com/YOUR_USERNAME/bayesian-optimization-capstone
```

**Copy this URL** - you'll submit it for your capstone project.

---

## Troubleshooting

### Problem: "Permission denied"

**Solution**: Make sure you're using a Personal Access Token, not your password.

### Problem: "Updates were rejected"

**Solution**: The remote has changes you don't have locally.

```bash
git pull origin main --rebase
git push origin main
```

### Problem: "fatal: not a git repository"

**Solution**: Make sure you're in the correct directory and ran `git init`.

```bash
cd /Users/hmoka2/Desktop/Work/GitHub_repos/Machine-Learning/Capstone_Project
git init
```

### Problem: Large file rejected

**Solution**: Check .gitignore is excluding large files. If needed:

```bash
# See what's being tracked
git ls-files

# If large files appear, add to .gitignore and remove from tracking
echo "large_file.csv" >> .gitignore
git rm --cached large_file.csv
git commit -m "Remove large file from tracking"
git push
```

### Problem: Notebook doesn't render

**Solution**: 
- Make sure file is valid JSON
- Check file size (GitHub has limits)
- Try re-saving notebook and pushing again

---

## Making Updates After Initial Push

If you need to make changes:

```bash
# Make your changes...

# Stage changes
git add .

# Commit changes
git commit -m "Describe your changes"

# Push to GitHub
git push
```

---

## Quick Reference Commands

```bash
# Check status
git status

# See what changed
git diff

# View commit history
git log --oneline

# See remote URL
git remote -v

# Pull latest changes
git pull

# Push changes
git push
```

---

## Success Criteria

✅ Repository is created on GitHub  
✅ All required files are visible  
✅ README displays correctly  
✅ Notebook renders properly  
✅ Repository is public  
✅ All links work  
✅ You have the submission URL  

---

## Your Submission URL

Once complete, submit this URL:

```
https://github.com/YOUR_USERNAME/bayesian-optimization-capstone
```

Replace `YOUR_USERNAME` with your actual GitHub username.

---

## Need More Help?

- Detailed guide: `docs/github_setup.md`
- Git basics: https://git-scm.com/doc
- GitHub docs: https://docs.github.com

---

**Good luck! 🚀**

**Estimated time**: 10-15 minutes for first-time setup

