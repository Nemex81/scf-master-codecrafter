---
description: >
scf_protected: false
scf_file_role: "prompt"
name: Release
scf_merge_priority: 20
scf_merge_strategy: "replace"
scf_version: "2.1.0"
spark: true
scf_owner: "scf-master-codecrafter"
---

Avvia Agent-Release, fornito da `spark-base`, per la versione ${input:Versione da rilasciare (es: v3.6.0)}.

Prerequisiti da verificare PRIMA di procedere:
1. Leggi CHANGELOG.md -- la sezione [UNRELEASED] e presente e completa?
2. Verifica che tutti i PLAN attivi abbiano status READY o siano completati
3. Verifica che docs/TODO.md non abbia fasi [ ] non completate

Se un prerequisito non e soddisfatto: blocca e comunica cosa manca.
Se tutti OK: segui il workflow dell'agente condiviso Agent-Release, fornito da `spark-base`.
