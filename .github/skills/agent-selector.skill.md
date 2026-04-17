---
description: Seleziona l'agente corretto in base a task, capability e plugin attivi.
scf_protected: false
scf_file_role: "skill"
name: agent-selector
scf_merge_priority: 20
scf_merge_strategy: "replace"
scf_version: "2.1.0"
spark: true
scf_owner: "scf-master-codecrafter"
---

# agent-selector

## Regole di selezione

- Setup o profilo progetto: Agent-Welcome.
- Orchestrazione multi-fase: Agent-Orchestrator.
- Git: Agent-Git.
- Documentazione framework: Agent-FrameworkDocs.
- Capability plugin disponibile: dispatcher corrispondente.
- Capability assente: Agent-Research.