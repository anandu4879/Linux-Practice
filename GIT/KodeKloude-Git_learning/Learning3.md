# Git Operations for DevOps Engineers - KodeKloud Notes

## Overview

These exercises teach real-world Git operations commonly performed by DevOps Engineers when managing repositories, collaborating with developers, and maintaining clean commit history.

---

# 1. Cherry-Pick a Specific Commit

## Scenario

A developer is working on a feature branch and wants only one specific commit merged into the master branch without merging the entire feature branch.

Commit Message:

```text
Update info.txt
```

## Concept

### What is Cherry-Pick?

Cherry-pick allows you to copy a specific commit from one branch and apply it to another branch.

Instead of:

```bash
git merge feature
```

which merges all commits, use:

```bash
git cherry-pick <commit-id>
```

to bring only one commit.

## Steps

### View commits on feature branch

```bash
git log feature --oneline
```

Example:

```text
7ab1234 Update info.txt
4cd5678 Added feature
```

### Switch to master

```bash
git checkout master
```

### Cherry-pick the commit

```bash
git cherry-pick 7ab1234
```

### Push changes

```bash
git push origin master
```

## Use Case

When only one bug fix or feature needs promotion without merging unfinished work.

---

# 2. Git Pull Request Workflow (Gitea)

## Scenario

Developers should not push directly to the master branch.

Changes must be:

1. Pushed to feature branch
2. Reviewed
3. Approved
4. Merged through Pull Request

## Workflow

### Developer (Max)

Create branch:

```bash
git checkout -b story/fox-and-grapes
```

Make changes:

```bash
git add .
git commit -m "Added fox story"
git push origin story/fox-and-grapes
```

### Create Pull Request

Source:

```text
story/fox-and-grapes
```

Destination:

```text
master
```

Title:

```text
Added fox-and-grapes story
```

### Add Reviewer

Reviewer:

```text
tom
```

### Reviewer Actions

1. Login as reviewer
2. Review code
3. Approve changes
4. Merge Pull Request

## Why Pull Requests?

Benefits:

* Code review
* Team collaboration
* Better quality control
* Protected master branch

---

# 3. Reset Repository History

## Scenario

Developers accidentally pushed test commits.

Need to restore repository to:

```text
initial commit
add data.txt file
```

and remove all later commits.

## Concept

### Hard Reset

Moves branch pointer backward.

```bash
git reset --hard <commit-id>
```

### Force Push

Updates remote repository with rewritten history.

```bash
git push --force
```

## Steps

Find commit:

```bash
git log --oneline
```

Reset:

```bash
git reset --hard <commit-id>
```

Verify:

```bash
git log --oneline
```

Push:

```bash
git push origin master --force
```

## Warning

Never force-push production branches unless absolutely necessary.

---

# 4. Clean Git Repository

## Scenario

Developers created unwanted files.

Need:

```text
working tree clean
```

without committing anything.

## Git Status

Check repository state:

```bash
git status
```

### Remove Modified Files

```bash
git restore .
```

Older Git:

```bash
git checkout -- .
```

### Remove Untracked Files

Preview:

```bash
git clean -fdn
```

Delete:

```bash
git clean -fd
```

### Verify

```bash
git status
```

Output:

```text
nothing to commit, working tree clean
```

## Difference

| Command       | Purpose                |
| ------------- | ---------------------- |
| git restore . | Restore tracked files  |
| git clean -fd | Remove untracked files |

---

# 5. Restore Stashed Changes

## Scenario

A developer saved unfinished work using Git stash.

Need to restore:

```text
stash@{1}
```

## Concept

### What is Git Stash?

Temporary storage for uncommitted changes.

Store changes:

```bash
git stash
```

View stashes:

```bash
git stash list
```

Example:

```text
stash@{0}
stash@{1}
stash@{2}
```

## Restore Specific Stash

```bash
git stash apply stash@{1}
```

## Verify

```bash
git status
```

## Commit Changes

```bash
git add .
git commit -m "Restore stashed changes"
```

## Push

```bash
git push origin master
```

## Apply vs Pop

| Command         | Result                   |
| --------------- | ------------------------ |
| git stash apply | Restore and keep stash   |
| git stash pop   | Restore and delete stash |

---

# Git Commands Cheat Sheet

## Branches

```bash
git branch
git branch -a
git checkout master
git checkout feature
git checkout -b new-branch
```

## Logs

```bash
git log
git log --oneline
git log --graph
```

## Stash

```bash
git stash
git stash list
git stash apply stash@{1}
git stash pop
```

## Cleaning

```bash
git restore .
git clean -fd
git status
```

## History Rewrite

```bash
git reset --hard HEAD~1
git reset --hard <commit-id>
git push --force
```

## Cherry-Pick

```bash
git cherry-pick <commit-id>
```

## Push

```bash
git push origin master
git push origin feature
```

## Pull Request Workflow

```text
Feature Branch
      ↓
Push
      ↓
Create PR
      ↓
Reviewer Approval
      ↓
Merge to Master
```

---

# DevOps Takeaways

A DevOps Engineer should understand:

* Branching strategies
* Cherry-picking commits
* Pull Request workflows
* Repository cleanup
* Stashing changes
* History rewriting
* Force pushing risks
* Collaboration through code reviews

These operations are frequently used in GitHub, GitLab, Bitbucket, Azure Repos, and Gitea environments.
