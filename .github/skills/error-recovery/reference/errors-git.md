---
spark: true
scf_file_role: "skill"
scf_version: "2.1.0"
scf_merge_strategy: "replace"
scf_protected: false
scf_owner: "scf-master-codecrafter"
scf_merge_priority: 20
---

# Error Recovery — Git

- working tree sporco: isolare prima le modifiche non correlate;
- merge bloccato: non procedere senza capire il conflitto;
- push negato: verificare branch, remote e policy prima di ritentare.