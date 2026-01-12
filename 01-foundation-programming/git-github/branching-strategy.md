# Branching Strategy

## Learning Objectives
- Understand branching models
- Master Git Flow
- Learn branch naming conventions
- Implement team workflow

## Topics

### 1. Why Branches?
- Isolated development
- Feature development
- Bug fixes
- Release management

### 2. Git Flow Model
- Main branch (production)
- Develop branch (staging)
- Feature branches
- Release branches
- Hotfix branches

### 3. Branch Naming
- feature/user-authentication
- bugfix/login-issue
- hotfix/critical-bug
- release/1.0.0

### 4. Working with Branches
- git branch
- git checkout / git switch
- git merge
- Handling conflicts

## Git Flow Workflow

```
main
  ↑
  |-- hotfix/critical-bug
  |       ↓
  |       (merge back)
  |
develop
  ↑
  |-- feature/new-feature
  |-- feature/user-auth
  |-- release/1.0.0
```

## Essential Commands

```bash
# Create and switch branch
git checkout -b feature/new-feature
git switch -c feature/new-feature

# List branches
git branch -a
git branch -D branch-name

# Merge branch
git checkout develop
git merge feature/new-feature

# Push branch
git push origin feature/new-feature
git push -u origin feature/new-feature

# Delete remote branch
git push origin --delete feature/new-feature
```

## Merge vs Rebase
```bash
# Merge - preserves history
git merge feature-branch

# Rebase - linear history
git rebase develop
```

## Practice Tasks
- [ ] Create feature branches
- [ ] Merge branches safely
- [ ] Handle merge conflicts
- [ ] Delete completed branches
- [ ] Implement team workflow

## Interview Tips
- Explain Git Flow model
- Know when to use merge vs rebase
- Understand branch protection rules
- Know conflict resolution

## Best Practices
- Delete merged branches
- Keep branches short-lived
- Use meaningful names
- Protect main/develop branches
- Require code review for merges

## Resources
- Atlassian: Git Flow
- Git Documentation: Branching
- GitHub Flow guide
