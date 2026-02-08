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












