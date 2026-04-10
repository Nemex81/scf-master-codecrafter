---
spark: true
---

# Changelog — scf-master-codecrafter

## [1.0.0] - 2026-04-10

### Added
- Prima release del layer master SCF.
- 7 agenti esecutori: Orchestrator v2.0, Git, Helper, Release, FrameworkDocs, Welcome, Research.
- 6 agenti dispatcher con fallback verso Agent-Research.
- Skill trasversali, instruction condivise e runtime state orchestratore.
- Supporto pattern multi-plugin M1 con `AGENTS-{plugin-id}.md`.

### Notes
- Richiede `spark-framework-engine >= 1.5.0`.