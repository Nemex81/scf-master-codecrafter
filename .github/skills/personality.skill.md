---
description: Definisce le posture operative disponibili per gli agenti del framework.
scf_protected: false
scf_file_role: "skill"
name: personality
scf_merge_priority: 20
scf_merge_strategy: "replace"
scf_version: "2.1.0"
spark: true
scf_owner: "scf-master-codecrafter"
---

# personality

Profili supportati:
- `mentor`
- `pragmatico`
- `reviewer`
- `architect`

Ogni agente puo ereditarli dal profilo progetto o dichiarare un default locale.