<p align="center">
  <img src="git.svg ">
</p>



# Git Pull Request Merge Strategies — Quick Notes

## 1. Merge Pull Request (Create a merge commit)

- Creates a **merge commit** (commit with **two parents**)
- Preserves **full branch history**
- Feature branch structure remains visible
- Commit hashes on feature branch **do not change**
- Easy to **revert the entire PR** with one revert
- Results in **non-linear (branched) history**
- Preferred for **backend, infra, audit-heavy repos**

**One-liner:**  
> PR merged using a **merge commit**

---

## 2. Rebase & Merge Pull Request

- **No merge commit** is created
- Feature branch commits are **rebased onto `main`**
- Commit hashes **change**
- Produces a **linear, clean history**
- Original branch structure is lost
- Reverting requires reverting **individual commits**
- Preferred for **frontend, docs, open-source repos**

**One-liner:**  
> PR **rebased and merged** (no merge commit)

---

## Quick Comparison

| Aspect | Merge Commit | Rebase & Merge |
|-----|-----|-----|
| Merge commit created | Yes | No |
| History | Branched | Linear |
| Commit hashes change | No | Yes |
| Easy full-PR revert | Yes | No |
| Audit trail | Strong | Weaker |
| Log readability | Medium | High |

---

## Memory Hook
- **Merge commit** → preserve history  
- **Rebase & merge** → clean history

---

# Git Conflicts and Conflict Resolution

## What is a Git Conflict?

A **Git conflict** occurs when Git cannot automatically merge changes because **the same part of a file has been modified differently in two branches**.

Instead of guessing which change is correct, Git pauses the operation and asks the user to **manually resolve the conflict**.

---

## When Do Git Conflicts Occur?

Conflicts commonly happen during:

- `git merge`
- `git rebase`
- `git pull`
- `git cherry-pick`

---

## Common Causes of Git Conflicts

- Two branches modify the **same line** in a file
- One branch **deletes a file** while another modifies it
- Long-lived branches diverging significantly from `main`
- Multiple developers working on the same file

---

## How Git Shows a Conflict

Git inserts conflict markers directly into the file:

````text
<<<<<<< HEAD (Current Change)
Code from the current branch
=======
Code from the incoming branch
>>>>>>> feature-branch (Incoming Change)

- HEAD → your current branch
- feature-branch → branch being merged or rebased
````

---

## How to Resolve a Git Conflict (Step-by-Step)

#### Step 1: Identify conflicted files
    - git status
    - Git will list files under: Unmerged paths

#### Step 2: Open and resolve the conflict
1. Open the conflicted file
2. Decide which changes to keep:
    - Keep current changes
    - Keep incoming changes
    - Combine both
3. Remove all conflict markers:
````text
<<<<<<<
=======
>>>>>>>
````
#### Step 3: Mark the conflict as resolved
git add <file-name> This tells Git the conflict has been fixed.

#### Step 4: Complete the operation
1. If resolving a merge conflict:  `git commit`

2. If resolving a rebase conflict: `git rebase --continue`

#### Aborting Conflict Resolution

* If you want to stop and go back to the previous state: 
    1. **Abort a Merge** : `git merge --abort`
    2. **Abort a rebase** : `git rebase --abort`

---
#### Best Practices to Avoid Git Conflicts

* Keep branches short-lived
* Pull latest changes frequently: `git pull --rebase`
* Avoid large commits touching many files
* Communicate when working on the same files
* Rebase feature branches before creating a Pull Request

## Interview One-Liner

**A Git conflict occurs when Git cannot automatically merge changes because the same lines were modified differently. Conflicts are resolved by manually editing the file, staging the fix, and completing the merge or rebase.**

---

# Git Reset vs Git Revert

## Overview

Both **`git reset`** and **`git revert`** are used to undo changes in Git, but they work in **very different ways** and are used in **different situations**.

- **`git reset`** rewrites history
- **`git revert`** preserves history

---

## git reset

### What it does
`git reset` moves the current branch pointer (`HEAD`) to a different commit and optionally changes the staging area and working directory.

It is a **history-rewriting operation**.

---

### Types of git reset

#### 1. Soft reset
```bash
git reset --soft <commit>
```

* Moves HEAD to the commit
* Keeps changes staged
* Working directory unchanged
* Use case: Fix or amend the last commit.

#### 2. Mixed reset (default)
```bash
git reset <commit> 
```

* Moves HEAD
* Keeps changes unstaged
* Working directory unchanged

#### 3. Hard reset
```bash
git reset --hard <commit>
```

* Moves HEAD
* Clears staging area
* Deletes changes from working directory

⚠️ Destructive operation
Use case: Discard local work completely.

#### Important rules for git reset

* Should be used only on local branches
* ❌ Do NOT use on shared or pushed branches
* Rewrites commit history

## git revert

What it does?
* git revert creates a new commit that reverses the changes made by an earlier commit.
* It does not rewrite history.

* Basic usage: 
```bash
git revert <commit>
```

* A new commit is created
* Original commit remains in history
* Safe for shared branches

#### Reverting a merge commit
```bash
git revert -m 1 <merge-commit>
```

* -m 1 specifies the main parent
* Required only for merge commits

#### When to use git revert

* Undo a commit that is already pushed
* Undo changes on main or shared branches
* When history integrity matters

#### Mental Model

* git reset → “Pretend this commit never happened”

* git revert → “Undo this commit safely”

#### Interview One-Liner

**`git reset` rewrites history and should be used only on local branches, whereas `git revert` creates a new commit that safely undoes changes and is suitable for shared branches.**


## Key Differences: git reset vs git revert

| Aspect | git reset | git revert |
|------|----------|-----------|
| Purpose | Removes commits by moving `HEAD` | Undoes a commit by creating a new commit |
| History | **Rewrites history** | **Preserves history** |
| New commit created | No | Yes |
| Commit removal | Commit is removed from history | Commit remains, effect is reversed |
| Safe for shared branches | ❌ No | ✅ Yes |
| Typical usage | Local cleanup and fixes | Undo changes on `main` / production |
| Risk level | High (can lose work) | Low (non-destructive) |
| Effect on collaborators | Can cause issues | Safe and predictable |
| Common commands | `--soft`, `--mixed`, `--hard` | `git revert <commit>` |

---












