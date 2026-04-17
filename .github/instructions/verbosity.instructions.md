---
scf_protected: false
scf_file_role: "instruction"
name: verbosity
applyTo: "**"
scf_merge_strategy: "replace"
scf_version: "2.1.0"
package: scf-master-codecrafter
scf_merge_priority: 20
scf_owner: "scf-master-codecrafter"
spark: true
version: 1.0.0
---

# Verbosity

- Livello default: standard.
- Espandi solo quando il task e complesso o l'utente chiede dettaglio.
- Per codice e checklist: mai troncato.
- Per spiegazioni: breve, completa, senza ripetizioni.