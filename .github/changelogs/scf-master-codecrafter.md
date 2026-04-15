---
spark: true
---

# Changelog — scf-master-codecrafter

## [2.0.0] - 2026-04-15

### Changed

- Nuovo file `.github/AGENTS-master.md` per dichiarare gli agenti CORE-CRAFT del pacchetto.
- Il pacchetto diventa un plugin CORE-CRAFT sopra `spark-base`.
- Il manifest dichiara `dependencies: ["spark-base"]` e mantiene solo design, routing e contesto MCP.

### Removed

- Agenti base, instruction condivise, skill general-purpose e prompt framework migrati a `spark-base`.

### Compatibility

- Richiede `spark-base >= 1.0.0` e `spark-framework-engine >= 1.9.0`.

## [1.0.0] - 2026-04-10

### Added

- Prima release del layer master SCF.
- 7 agenti esecutori: Orchestrator v2.0, Git, Helper, Release, FrameworkDocs, Welcome, Research.
- 6 agenti dispatcher con fallback verso Agent-Research.
- Skill trasversali, instruction condivise e runtime state orchestratore.
- Supporto pattern multi-plugin M1 con `AGENTS-{plugin-id}.md`.

### Notes

- Richiede `spark-framework-engine >= 1.9.0`.
