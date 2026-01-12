# Commit Messages

## Learning Objectives
- Write clear commit messages
- Follow conventions
- Maintain git history quality
- Communicate changes effectively

## Topics

### 1. Importance of Good Commits
- Code history documentation
- Debugging and blame
- Understanding changes
- Project communication

### 2. Commit Message Format
```
<type>(<scope>): <subject>

<body>

<footer>
```

### 3. Types
- **feat**: New feature
- **fix**: Bug fix
- **docs**: Documentation
- **style**: Code style (no logic change)
- **refactor**: Code refactoring
- **perf**: Performance improvement
- **test**: Test addition/update
- **chore**: Build, deps, tooling

### 4. Subject Line Rules
- 50 characters or less
- Imperative mood ("add" not "added")
- Capitalize first letter
- No period at end

## Examples

```bash
# Good ✅
git commit -m "feat(auth): add user login functionality"
git commit -m "fix(api): handle null response in getUser"
git commit -m "docs: update installation instructions"
git commit -m "refactor(utils): simplify helper functions"

# Bad ❌
git commit -m "update code"
git commit -m "Fixed bug."
git commit -m "random changes"
git commit -m "wip"
```

## Full Example with Body
```
feat(checkout): add payment method selection

- Add PayPal and Stripe options
- Implement payment validation
- Update checkout flow

Closes #123
Relates to #456
```

## Conventional Commits
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Tips for Good Commits
1. Commit frequently with small changes
2. One logical change per commit
3. Write clear, descriptive messages
4. Use present tense
5. Reference issues/tickets
6. Include "why", not just "what"

## Practice Tasks
- [ ] Write commits following conventions
- [ ] Write commits with body
- [ ] Reference issues properly
- [ ] Review git log quality
- [ ] Amend commit messages when needed

## Tools
- commitlint: Enforce format
- husky: Pre-commit hooks
- conventional-changelog: Auto changelog

## Interview Tips
- Know commit conventions
- Explain message importance
- Discuss squashing commits
- Understand git history value

## Resources
- Conventional Commits
- Angular Commit Guidelines
- Seven Rules of Great Git Commits
