# Git Basics

## Learning Objectives
- Master essential Git commands
- Understand repository structure
- Learn staging and committing
- Work with remote repositories

## Topics

### 1. Git Fundamentals
- What is Git?
- Repository initialization
- Working directory, staging area, repository
- Git configuration

### 2. Basic Commands
- git init
- git add
- git commit
- git status
- git log
- git diff

### 3. Undoing Changes
- git restore
- git revert
- git reset
- Choosing the right approach

### 4. Remote Repositories
- git remote
- git push
- git pull
- git fetch

## Essential Commands

```bash
# Setup
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# Create and commit
git init
git add .
git commit -m "Initial commit"

# Check status
git status
git log --oneline

# Undo changes
git restore <file>          # Discard changes
git reset HEAD~1            # Undo last commit
git revert <commit>         # Create new commit undoing changes

# Remote operations
git remote add origin <url>
git push origin main
git pull origin main
```

## Commit Message Format
```
<type>: <subject>

<body>

<footer>
```

Types: feat, fix, docs, style, refactor, test, chore

## Practice Tasks
- [ ] Create local repository
- [ ] Make meaningful commits
- [ ] Practice undoing changes
- [ ] Connect to remote repository
- [ ] Push and pull changes

## Interview Tips
- Know basic Git workflow
- Explain staging area purpose
- Understand commit structure
- Know difference between reset and revert

## Resources
- Git Documentation
- GitHub Learning Lab
- Atlassian Git Tutorials
