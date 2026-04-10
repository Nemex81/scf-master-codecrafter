# SCF Master CodeCrafter

**Layer base trasversale per tutti i plugin linguaggio-specifici del SPARK Code Framework.**

## Cosè

`scf-master-codecrafter` è il layer fondamentale del SPARK Code Framework (SCF). Non è un plugin generico: è il coordinatore centrale che gestisce tutto ciò che è indipendente dal linguaggio di programmazione.

Ogni plugin linguaggio-specifico (es. `scf-pycode-crafter` per Python) dichiara `scf-master-codecrafter` come dipendenza obbligatoria.

## Cosa fa

- **Orchestrazione autonoma E2E** — `Agent-Orchestrator` v2.0 con `execution_mode`, `confidence_threshold` e loop autonomo
- **Routing dinamico** — agenti dispatcher che delegano ai plugin specializzati o fanno fallback via `Agent-Research`
- **Git e Release** — `Agent-Git` e `Agent-Release` trasversali a qualsiasi linguaggio
- **Documentazione framework** — binario separato dal binario progetto
- **Runtime state** — `.github/runtime/orchestrator-state.json` esposto via MCP tools del motore

## Architettura

```
scf-master-codecrafter (questo package)
├── Agenti esecutori (7): Orchestrator, Git, Helper, Release, FrameworkDocs, Welcome, Research
├── Agenti dispatcher (6): CodeRouter, Analyze, Design, Plan, Docs, CodeUI
├── Skill trasversali (24)
├── Instructions trasversali (6)
└── Runtime state: .github/runtime/orchestrator-state.json

plugin linguaggio (es. scf-pycode-crafter)
├── dependencies: ["scf-master-codecrafter"]
├── Agenti specializzati con frontmatter capabilities
└── AGENTS-{plugin-id}.md (scoperto da engine via glob)
```

## Requisiti

- **SPARK Framework Engine** ≥ `1.5.0`
- **Python** ≥ 3.10 (per il motore MCP)

## Installazione

```
scf_install_package("scf-master-codecrafter")
```

Se stai installando un plugin linguaggio-specifico, installa prima questo package o lascia che il motore risolva la dipendenza automaticamente.

## Plugin compatibili

| Package | Versione | Linguaggio |
|---|---|---|
| `scf-pycode-crafter` | ≥ 2.0.0 | Python |

## Maintainer

Nemex81
