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
- Agent-Research — support/internal — fallback research, unknown-stack briefing
- Agent-CodeRouter — dispatcher — code, code-ui, routing
- Agent-Analyze — dispatcher — analyze
- Agent-Design — dispatcher — design
- Agent-Plan — dispatcher — plan
- Agent-Docs — dispatcher — docs
- Agent-CodeUI — dispatcher — code-ui, ui

## Plugin Agents

Questa sezione viene popolata dai plugin installati tramite file `AGENTS-{plugin-id}.md`.
Il motore aggrega i file disponibili tramite `scf://agents-index`.

## MCP Runtime Tools (engine v1.5.0+ — patch non ancora applicata)

> ⚠️ Questa sezione è anticipatoria. I tool seguenti saranno disponibili dopo
> l'applicazione della patch Step 2 a `spark-framework-engine.py`.
> Stato attuale del motore: v1.4.2.

- `scf_get_runtime_state()` — legge lo stato runtime dell'orchestratore
- `scf_update_runtime_state(patch)` — aggiorna lo stato runtime dell'orchestratore
- `scf://runtime-state` — resource JSON con lo stato runtime corrente
- `scf://agents-index` — aggrega `AGENTS.md` e `AGENTS-{plugin-id}.md`

***

## Agenti di Supporto Interno

Questi agenti non fanno parte del workflow principale ANALYZE→RELEASE.
Vengono invocati automaticamente da altri agenti in condizioni specifiche.
L'utente non li chiama direttamente.

### Agent-Research

- **Ruolo**: fallback per linguaggi senza plugin SCF specializzato
- **Visibilità**: internal
- **Invocato da**: Agent-Analyze, Agent-Design, Agent-Plan, Agent-CodeUI, Agent-Docs
- **Produce**: context brief in `.github/runtime/research-cache/{language}-{task-type}.md`
- **Limite**: non sostituisce un plugin testato — fallback trasparente dichiarato