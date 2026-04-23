---
description: >
scf_protected: false
scf_file_role: "prompt"
name: Sync Documentazione
scf_merge_priority: 20
scf_merge_strategy: "replace"
scf_version: "2.1.0"
spark: true
scf_owner: "scf-master-codecrafter"
---

Avvia Agent-Docs, fornito da `spark-base`, per sincronizzare la documentazione dopo i commit recenti.

Contesto da analizzare:
- File modificati: ${input:Elenca i file .py modificati (separati da virgola)}
- Tipo di modifica: ${input:Tipo (feat/fix/refactor)}
- Versione target: ${input:Versione (es: v3.6.0)}

Segui la Sync Strategy dell'agente condiviso Agent-Docs, fornito da `spark-base`.
Produci al termine la Sync Checklist con stato di ogni documento.
