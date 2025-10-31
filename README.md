# gitflow-advices
Quite structured and already close to a mature enterprise branching model (a mix between GitHub Flow and Gitflow).

## Best Practices

### Feature Branches: Use Rebase
Use rebases on feature branches to keep a clean, readable timeline. This approach:
- Maintains a linear, easy-to-follow commit history
- Makes it easier to understand the evolution of features
- Simplifies debugging with tools like `git bisect`
- Reduces merge commit clutter in feature development

**When to rebase:**
- Before merging a feature branch into the main branch
- To incorporate upstream changes from main into your feature branch
- To clean up local commits before sharing

**Example workflow:**
```bash
# Update your feature branch with latest main changes
git checkout feature/my-feature
git fetch origin
git rebase origin/main

# Clean up your commits if needed
git rebase -i origin/main
```

### Shared Branches: Use Merge
Use merges between shared branches (main → production, hotfix → main) to preserve history integrity. This approach:
- Maintains a complete audit trail of when changes were integrated
- Preserves branch topology and context
- Avoids rewriting shared history that others depend on
- Shows clear integration points between major branches

**When to merge:**
- Integrating changes from main to production
- Merging hotfixes back into main
- Combining long-lived branches
- Any integration involving branches that others are working on

**Example workflow:**
```bash
# Merge main into production
git checkout production
git merge main --no-ff

# Merge hotfix back into main
git checkout main
git merge hotfix/critical-fix --no-ff
```

### Pull Requests: Always Required
Always open PRs for controlled review, testing, and audit trail. This practice:
- Enables code review and knowledge sharing
- Triggers automated testing and CI/CD pipelines
- Creates a documented history of changes and discussions
- Provides a gate for quality control and approval processes

**PR workflow:**
1. Create a feature branch from main
2. Make your changes with clear, atomic commits
3. Rebase on main to ensure a clean history
4. Push your branch and open a PR
5. Address review feedback
6. Wait for approvals and passing tests
7. Merge (or have a maintainer merge) the PR

**Example:**
```bash
# Create and work on feature branch
git checkout -b feature/new-feature main
# ... make changes ...
git add .
git commit -m "Add new feature"

# Before PR, rebase on main
git fetch origin
git rebase origin/main

# Push and create PR
git push origin feature/new-feature
# Open PR via GitHub UI or CLI
```

## Summary
- **Feature branches**: Use `rebase` for clean, linear history
- **Shared branches**: Use `merge` to preserve integration history
- **All changes**: Go through Pull Requests for review and audit trail
