# Github Commands

## Change to another branch

```bash
git checkout branch-name
```

# To commit a change from another branch to this branch

```bash
git checkout branch-you-want-to-get-the-commit-sha-from
git log
git checkout branch-you-want-to-apply-the-cherrypick-to
git cherry-pick commit-sha
```

# To revert a commit on branch a

```bash
git checkout branch-a
git log
git revert commit-sha
```
