---
description: Controlla che il task resti nel perimetro richiesto e segnala deviazioni prima di implementare.
scf_protected: false
scf_file_role: "skill"
name: task-scope-guard
scf_merge_priority: 20
scf_merge_strategy: "replace"
scf_version: "2.1.0"
spark: true
scf_owner: "scf-master-codecrafter"
---

# task-scope-guard

- Esplicita repository e file in scope.
- Se emergono side effect cross-repo, dichiarali prima di modificare.
- Non estendere il task senza motivo verificabile.