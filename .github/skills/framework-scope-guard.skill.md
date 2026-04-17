---
description: Protegge il layer master da richieste fuori scope o non coperte dai plugin installati.
scf_protected: false
scf_file_role: "skill"
name: framework-scope-guard
scf_merge_priority: 20
scf_merge_strategy: "replace"
scf_version: "2.1.0"
spark: true
scf_owner: "scf-master-codecrafter"
---

# framework-scope-guard

- Se il task richiede competenze non presenti, non improvvisare: instrada ad Agent-Research.
- Se il task chiede modifiche fuori perimetro framework, esplicitalo prima di procedere.
- Se un dispatcher non trova capability compatibili, ferma l'esecuzione automatica e segnala il fallback.