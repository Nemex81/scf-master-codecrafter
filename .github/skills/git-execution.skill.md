---
description: Matrice sintetica dei comandi git consentiti, vietati e delegati ad Agent-Git.
scf_protected: false
scf_file_role: "skill"
name: git-execution
scf_merge_priority: 20
scf_merge_strategy: "replace"
scf_version: "2.1.0"
spark: true
scf_owner: "scf-master-codecrafter"
---

# git-execution

## Consentiti read-only

- `git status`
- `git diff`
- `git log`
- `git show`

## Consentiti solo via Agent-Git

- `git commit`
- `git push`
- `git merge`
- `git tag`

## Vietati in autonomia

- `git reset --hard`
- `git rebase`
- force push su branch condivisi