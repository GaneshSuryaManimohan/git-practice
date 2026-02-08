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