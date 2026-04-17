---
description: >
scf_protected: false
scf_file_role: "prompt"
name: Start / Riprendi Codifica
scf_merge_priority: 20
scf_merge_strategy: "replace"
scf_version: "2.1.0"
spark: true
scf_owner: "scf-master-codecrafter"
---

Leggi il file docs/TODO.md corrente.

1. Identifica la prima voce con checkbox non spuntata [ ]
2. Leggi il PLAN collegato (indicato in TODO.md come "Piano di riferimento")
3. Comunica: "Riprendo da [Fase X]: [descrizione fase]"
4. Avvia Agent-Code per quella fase specifica

Se docs/TODO.md non esiste, comunicalo e suggerisci di usare #init.prompt.md
per creare un nuovo task.
