---
spark: true
---

# AGENTS Index

## Master Agents (scf-master-codecrafter)

- Agent-Orchestrator — executor — orchestration, workflow, runtime-state
- Agent-Git — executor — git, commit, push, merge, tag proposal
- Agent-Helper — executor — framework-help, discovery, routing hints
- Agent-Release — executor — release-coordination, semver, packaging guidance
- Agent-FrameworkDocs — executor — framework-docs, changelog, AGENTS index
- Agent-Welcome — executor — setup, project-profile, onboarding
- Agent-Research — executor — fallback research, unknown-stack briefing
- Agent-CodeRouter — dispatcher — code, code-ui, routing
- Agent-Analyze — dispatcher — analyze
- Agent-Design — dispatcher — design
- Agent-Plan — dispatcher — plan
- Agent-Docs — dispatcher — docs
- Agent-CodeUI — dispatcher — code-ui, ui

## Plugin Agents

Questa sezione viene popolata dai plugin installati tramite file `AGENTS-{plugin-id}.md`.
Il motore aggrega i file disponibili tramite `scf://agents-index`.

## MCP Runtime Tools (engine v1.5.0+)

- `scf_get_runtime_state()` — legge lo stato runtime dell'orchestratore
- `scf_update_runtime_state(patch)` — aggiorna lo stato runtime dell'orchestratore
- `scf://runtime-state` — resource JSON con lo stato runtime corrente
- `scf://agents-index` — aggrega `AGENTS.md` e `AGENTS-{plugin-id}.md`