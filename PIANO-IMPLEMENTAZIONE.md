# Piano di Implementazione — scf-master-codecrafter

> **Destinatario**: Copilot in Agent mode  
> **Data**: 2026-04-10  
> **Stato**: scaffold pronto, implementazione da completare  
> **Workspace multi-root richiesto**: `scf-master-codecrafter`, `spark-framework-engine`, `scf-pycode-crafter`

---

## Contesto Architetturale

Questo repo è il **layer base** del SPARK Code Framework. Ogni plugin linguaggio-specifico dichiara questo package come dipendenza. Il motore MCP (`spark-framework-engine`) deve essere aggiornato alla v1.5.0 prima che questo package sia installabile (vedi Fase A).

I tre repo coinvolti nel workspace:
- `spark-framework-engine` — motore MCP (da patchare a v1.5.0)
- `scf-master-codecrafter` — questo repo (da popolare)
- `scf-pycode-crafter` — plugin Python (da refactorare a v2.0.0)

---

## Vincoli Invarianti

1. Tutti i file markdown SCF devono avere `spark: true` nel frontmatter YAML
2. NON modificare `ManifestManager` o il package system dell'engine
3. NON toccare `.github/runtime/` con il ManifestManager — quella directory è runtime, non manifest
4. `file_ownership_policy: "error"` su tutti i manifest — nessun merge automatico
5. Ogni agente deve avere frontmatter completo: `name`, `version`, `spark`, `layer`
6. Agenti dispatcher devono avere: `role: dispatcher`, `delegates_to_capabilities`, `fallback`
7. Agenti plugin devono avere: `plugin`, `capabilities`, `languages`
8. Output strutturato NVDA-compatibile: prefisso `ERRORE:` per errori critici, output testuale navigabile
9. NON eseguire `git push` direttamente — sempre via Agent-Git
10. NON modificare `FRAMEWORK_CHANGELOG.md` con Agent-Docs (scope esclusivo Agent-FrameworkDocs)

---

## Fase A — Patch `spark-framework-engine` → v1.5.0

**File da modificare**: `spark-framework-engine/spark-framework-engine.py`

### A1. Bump versione
```python
ENGINE_VERSION: str = "1.5.0"  # era 1.4.2
```

### A2. Nuovo metodo in `FrameworkInventory` — dopo `get_package_changelog`

```python
def list_agents_indexes(self) -> list[FrameworkFile]:
    """Scopre tutti i file AGENTS*.md in .github/ root.
    Supporta il pattern multi-plugin M1: master possiede AGENTS.md,
    ogni plugin possiede AGENTS-{plugin-id}.md.
    """
    if not self._ctx.github_root.is_dir():
        return []
    return sorted(
        [
            self._build_framework_file(p, "index")
            for p in self._ctx.github_root.glob("AGENTS*.md")
            if p.is_file()
        ],
        key=lambda ff: ff.name,
    )

def get_orchestrator_state(self) -> dict:
    """Leggi stato runtime orchestratore da .github/runtime/orchestrator-state.json.
    Restituisce stato di default se il file non esiste.
    Restituisce {} con warning se il file è corrotto.
    .github/runtime/ NON viene tracciato dal ManifestManager.
    """
    state_path = self._ctx.github_root / "runtime" / "orchestrator-state.json"
    if not state_path.is_file():
        return {
            "current_phase": "",
            "current_agent": "",
            "retry_count": 0,
            "confidence": 1.0,
            "execution_mode": "autonomous",
            "last_updated": "",
            "phase_history": [],
            "active_task_id": "",
        }
    try:
        return json.loads(state_path.read_text(encoding="utf-8"))
    except (OSError, json.JSONDecodeError) as exc:
        _log.warning("orchestrator-state.json unreadable: %s", exc)
        return {}

def set_orchestrator_state(self, patch: dict) -> dict:
    """Aggiorna orchestrator-state.json con merge parziale.
    Crea .github/runtime/ se non esiste.
    Aggiorna last_updated con timestamp UTC ISO-8601.
    Restituisce lo stato completo post-aggiornamento.
    """
    state_path = self._ctx.github_root / "runtime" / "orchestrator-state.json"
    state_path.parent.mkdir(parents=True, exist_ok=True)
    current = self.get_orchestrator_state()
    current.update(patch)
    current["last_updated"] = datetime.now(timezone.utc).strftime("%Y-%m-%dT%H:%M:%SZ")
    state_path.write_text(
        json.dumps(current, indent=2, ensure_ascii=False),
        encoding="utf-8",
    )
    return current
```

### A3. Aggiornamento resource `scf://agents-index` in `register_resources()`

Sostituire il blocco esistente della resource `scf://agents-index` con:

```python
@self._mcp.resource("scf://agents-index")
async def resource_agents_index() -> str:
    indexes = inventory.list_agents_indexes()
    if not indexes:
        return "AGENTS.md not found."
    parts = []
    for ff in indexes:
        parts.append(ff.path.read_text(encoding="utf-8", errors="replace"))
    return "\n\n---\n\n".join(parts)
```

### A4. Nuovi tool in `register_tools()` — in fondo, prima della fine del metodo

```python
@self._mcp.tool()
async def scf_get_runtime_state() -> dict:
    """Leggi lo stato runtime dell'orchestratore da .github/runtime/orchestrator-state.json.
    Restituisce stato corrente o default se il file non esiste.
    Usare prima di ogni fase autonoma per verificare execution_mode e confidence.
    """
    return inventory.get_orchestrator_state()

@self._mcp.tool()
async def scf_update_runtime_state(patch: dict) -> dict:
    """Aggiorna campi selezionati di orchestrator-state.json (merge parziale).
    Accetta solo i campi da modificare. Aggiorna last_updated automaticamente.
    Crea .github/runtime/ se non esiste.
    Esempio: {"current_phase": "CODE", "confidence": 0.92, "current_agent": "Agent-Code"}
    Restituisce lo stato completo post-aggiornamento.
    """
    return inventory.set_orchestrator_state(patch)
```

### A5. Nuova resource in `register_resources()` — prima del _log.info finale

```python
@self._mcp.resource("scf://runtime-state")
async def resource_runtime_state() -> str:
    """Stato runtime orchestratore come JSON formattato.
    Leggibile direttamente da Agent mode senza chiamare il tool.
    """
    state = inventory.get_orchestrator_state()
    return json.dumps(state, indent=2, ensure_ascii=False)
```

### A6. Aggiornare il log finale di `register_resources()`

```python
# da:
_log.info("Resources registered: 4 list + 4 template + 6 scf:// singletons (14 total)")
# a:
_log.info("Resources registered: 4 list + 4 template + 7 scf:// singletons (15 total)")
```

### A7. Aggiornare il docstring di `register_tools()`

```python
# da: """Register all 23 MCP tools."""
# a:  """Register all 25 MCP tools."""
```

### A8. Aggiungere entry CHANGELOG in `spark-framework-engine/CHANGELOG.md`

Nuova sezione in testa al file:

```markdown
## [1.5.0] - 2026-04-10

### Added
- `scf_get_runtime_state` tool: legge `.github/runtime/orchestrator-state.json`
- `scf_update_runtime_state` tool: merge parziale su orchestrator-state.json
- `scf://runtime-state` resource: stato runtime come JSON leggibile da Agent mode
- `FrameworkInventory.get_orchestrator_state()`: lettura con default e gestione errori
- `FrameworkInventory.set_orchestrator_state()`: scrittura con merge, mkdir, timestamp UTC
- `FrameworkInventory.list_agents_indexes()`: glob `AGENTS*.md` per pattern multi-plugin M1
- `scf://agents-index` resource aggiornata: vista aggregata di tutti i file AGENTS*.md

### Changed
- Tool count: 23 → 25
- Resource count: 14 → 15
- `scf://agents-index`: da singolo file a concatenazione di tutti AGENTS*.md presenti

### Notes
- `.github/runtime/` non viene tracciato dal ManifestManager — comportamento by design
- `min_engine_version` richiesta da scf-master-codecrafter: 1.5.0
```

---

## Fase B — Popolare `scf-master-codecrafter`

Tutti i file elencati in `package-manifest.json` devono essere creati.
Riferimento fonti: leggere i file corrispondenti in `scf-pycode-crafter` come base,
adattare rimuovendo tutto ciò che è Python-specific.

### B1. File di progetto root

**`.github/project-profile.md`** — profilo neutro, senza language:
```yaml
---
spark: true
initialized: false
active_plugins: []
framework_version: ""
---
```
Corpo: istruzioni per compilare il profilo al setup.

**`.github/copilot-instructions.md`** — istruzioni base framework, NON Python-specific.
Base: leggere `.github/copilot-instructions.md` in `scf-pycode-crafter`, rimuovere
tutto ciò che fa riferimento a Python, pytest, ruff, mypy, type hints.

**`.github/AGENTS.md`** — indice master. Struttura:
```markdown
# AGENTS Index

## Master Agents (scf-master-codecrafter)
[lista agenti esecutori e dispatcher con nome, ruolo, capabilities]

## Plugin Agents
[sezione vuota con nota: "Populated by installed plugins via AGENTS-{plugin-id}.md"]

## MCP Runtime Tools (engine v1.5.0+)
- scf_get_runtime_state() — leggi stato orchestratore
- scf_update_runtime_state(patch) — aggiorna stato orchestratore
- scf://runtime-state — resource stato runtime
```

### B2. Agenti esecutori (7 file in `.github/agents/`)

Frontmatter obbligatorio per tutti gli agenti esecutori:
```yaml
---
spark: true
name: Agent-{Nome}
version: 1.0.0
layer: master
role: executor
---
```

**`Agent-Orchestrator.md`** — frontmatter esteso:
```yaml
---
spark: true
name: Agent-Orchestrator
version: 2.0.0
layer: master
role: executor
execution_mode: autonomous
confidence_threshold: 0.85
checkpoints: [design-approval, plan-approval, release]
runtime_state_tool: scf_get_runtime_state
runtime_update_tool: scf_update_runtime_state
---
```
Corpo: flusso E2E con loop autonomo, gestione confidence, retry max 2,
post-step analysis, riduzione checkpoint a 3.
Base: leggere `py-Agent-Orchestrator.md` in `scf-pycode-crafter`, promuovere
a versione trasversale e aggiungere execution_mode + confidence system.

**`Agent-Git.md`** — base: `py-Agent-Git.md` in pycode-crafter, rimuovere Python-specific.
**`Agent-Helper.md`** — base: `py-Agent-Helper.md`, rimuovere Python-specific.
**`Agent-Release.md`** — base: `py-Agent-Release.md`, rimuovere Python-specific.
**`Agent-FrameworkDocs.md`** — gestisce FRAMEWORK_CHANGELOG.md e docs del framework stesso.
**`Agent-Welcome.md`** — onboarding progetto, istruzioni primo setup.
**`Agent-Research.md`** — NUOVO, nessuna base in pycode:
```yaml
---
spark: true
name: Agent-Research
version: 1.0.0
layer: master
role: executor
visibility: internal
output_path: .github/runtime/research-cache/
---
```
Corpo: ricerca online su linguaggio/framework sconosciuto, produce brief strutturato
in `research-cache/{language}-{task-type}.md`. Dichiara sempre che l'output
è fallback dinamico, non competenza nativa. Non è user-facing.

### B3. Agenti dispatcher (6 file in `.github/agents/`)

Frontmatter obbligatorio:
```yaml
---
spark: true
name: Agent-{Nome}
version: 1.0.0
layer: master
role: dispatcher
delegates_to_capabilities: [{capability}]
fallback: Agent-Research
---
```

Meccanismo dispatcher (corpo comune per tutti):
1. Leggi `project-profile.md` → campo `active_plugins`
2. Leggi tutti i file `AGENTS*.md` via MCP tool `scf://agents-index` → mappa capabilities
3. SE esiste agente con capability richiesta → delega con contesto completo
4. SE non esiste → chiama `Agent-Research` → usa il brief prodotto come contesto

Capabilities per ciascun dispatcher:
- `Agent-CodeRouter` — `delegates_to_capabilities: [code, code-ui, routing]`
- `Agent-Analyze` — `delegates_to_capabilities: [analyze]`
- `Agent-Design` — `delegates_to_capabilities: [design]`
- `Agent-Plan` — `delegates_to_capabilities: [plan]`
- `Agent-Docs` — `delegates_to_capabilities: [docs]`
- `Agent-CodeUI` — `delegates_to_capabilities: [code-ui, ui]`

Base per tutti: leggere i corrispondenti `py-Agent-*.md` in pycode-crafter,
rimuovere Python-specific, aggiungere meccanismo dispatcher.

### B4. Instructions (6 file in `.github/instructions/`)

Base: leggere i corrispondenti file in `scf-pycode-crafter/.github/instructions/`.
Rimuovere tutto ciò che è Python-specific da ogni file.
File da non portare nel master: `python.instructions.md`, `tests.instructions.md`,
`project-reset.instructions.md` (valutare se generica, se sì portarla).

### B5. Skill (24+ file in `.github/skills/`)

Base per tutte le skill trasversali: leggere i corrispondenti file in `scf-pycode-crafter`.
Rimuovere riferimenti Python-specific.
File da rimuovere se Python-specific: `error-recovery/reference/errors-python.md`
(resta nel plugin Python).

Skill `framework-index/SKILL.md` e `framework-scope-guard.skill.md` — NUOVE,
non presenti in pycode-crafter, da creare da zero:
- `framework-index`: catalogo navigabile di tutti gli agenti e skill del framework installato
- `framework-scope-guard`: protezione perimetro framework, evita modifiche fuori scope

### B6. Runtime state default

**`.github/runtime/orchestrator-state.json`**:
```json
{
  "current_phase": "",
  "current_agent": "",
  "retry_count": 0,
  "confidence": 1.0,
  "execution_mode": "autonomous",
  "last_updated": "",
  "phase_history": [],
  "active_task_id": ""
}
```

### B7. Changelog package

**`.github/changelogs/scf-master-codecrafter.md`**:
```markdown
# Changelog — scf-master-codecrafter

## [1.0.0] - 2026-04-10

### Added
- Prima release del layer master SCF
- 7 agenti esecutori: Orchestrator v2.0, Git, Helper, Release, FrameworkDocs, Welcome, Research
- 6 agenti dispatcher con meccanismo fallback via Agent-Research
- 24 skill trasversali migrate da scf-pycode-crafter
- 6 instruction files trasversali
- Runtime state orchestratore: .github/runtime/orchestrator-state.json
- Supporto pattern multi-plugin M1: AGENTS-{plugin-id}.md per plugin
- min_engine_version: 1.5.0 (richiede scf_get_runtime_state e scf_update_runtime_state)
```

---

## Fase C — Refactor `scf-pycode-crafter` → v2.0.0

### C1. Creare `.github/AGENTS-python.md`

Indice degli agenti Python. Struttura:
```markdown
# AGENTS — scf-pycode-crafter (Python)

## Agenti specializzati Python

| Nome | Capabilities | Linguaggi |
|---|---|---|
| py-Agent-Code | code, implementation | python |
| py-Agent-Analyze | analyze, code-review, refactor, type-check | python |
| py-Agent-Design | design, architecture | python |
| py-Agent-Plan | plan | python |
| py-Agent-Validate | validate, test, lint | python |
```

### C2. Creare `.github/python.profile.md`

Profilo tecnico Python del progetto:
```yaml
---
spark: true
plugin: scf-pycode-crafter
---
```
Corpo: stack Python (interprete, testing, linting, type checker), convenzioni specifiche.

### C3. Aggiornare frontmatter di tutti gli agenti rimanenti

Aggiungere a ogni `py-Agent-*.md`:
```yaml
plugin: scf-pycode-crafter
capabilities: [...]   # vedi lista per ciascuno
languages: [python]
```

Capabilities per ciascun agente:
- `py-Agent-Code`: `[code, implementation]`
- `py-Agent-Analyze`: `[analyze, code-review, refactor, type-check]`
- `py-Agent-Design`: `[design, architecture]`
- `py-Agent-Plan`: `[plan]`
- `py-Agent-Validate`: `[validate, test, lint]`

### C4. Rimuovere i file migrati al master

File da eliminare dal repo `scf-pycode-crafter` (già posseduti dal master):
- `.github/agents/py-Agent-Orchestrator.md` (Opzione A: rimosso, non sostituito)
- `.github/agents/py-Agent-Git.md`
- `.github/agents/py-Agent-Helper.md`
- `.github/agents/py-Agent-Release.md`
- `.github/agents/py-Agent-CodeRouter.md`
- `.github/agents/py-Agent-Docs.md`
- `.github/AGENTS.md` → sostituito da `AGENTS-python.md`
- `.github/copilot-instructions.md`
- `.github/project-profile.md`
- Tutte le skill trasversali (vedi lista in package-manifest.json del master)
- Tutte le instructions trasversali (framework-guard, git-policy, model-policy, personality, verbosity, workflow-standard)

### C5. Aggiornare `package-manifest.json` di pycode-crafter

```json
{
  "schema_version": "2.0",
  "package": "scf-pycode-crafter",
  "version": "2.0.0",
  "display_name": "SCF Python Code Crafter",
  "description": "Agenti, skill e istruzioni specializzati per lo sviluppo Python. Richiede scf-master-codecrafter.",
  "author": "Nemex81",
  "min_engine_version": "1.5.0",
  "dependencies": ["scf-master-codecrafter"],
  "conflicts": [],
  "file_ownership_policy": "error",
  "changelog_path": ".github/changelogs/scf-pycode-crafter.md",
  "files": [
    ".github/AGENTS-python.md",
    ".github/python.profile.md",
    ".github/changelogs/scf-pycode-crafter.md",
    ".github/agents/py-Agent-Code.md",
    ".github/agents/py-Agent-Analyze.md",
    ".github/agents/py-Agent-Design.md",
    ".github/agents/py-Agent-Plan.md",
    ".github/agents/py-Agent-Validate.md",
    ".github/instructions/python.instructions.md",
    ".github/instructions/tests.instructions.md",
    ".github/skills/error-recovery/reference/errors-python.md"
  ]
}
```

---

## Fase D — Aggiornare `scf-registry`

**File**: `scf-registry/registry.json`

Aggiungere nel array `packages`:
```json
{
  "id": "scf-master-codecrafter",
  "display_name": "SCF Master CodeCrafter",
  "description": "Layer base prerequisito per tutti i plugin SCF linguaggio-specifici.",
  "latest_version": "1.0.0",
  "status": "stable",
  "repo_url": "https://github.com/Nemex81/scf-master-codecrafter",
  "engine_min_version": "1.5.0",
  "tags": ["master", "orchestrator", "dispatcher", "base"]
}
```

Aggiornare entry esistente `scf-pycode-crafter`:
- `latest_version`: `"1.2.1"` → `"2.0.0"`
- Aggiungere `"engine_min_version": "1.5.0"`

---

## Checklist Finale per Copilot

Prima di considerare il lavoro completo, verificare:

- [ ] `spark-framework-engine.py` versione è `1.5.0`
- [ ] Tool count nel docstring di `register_tools()` è 25
- [ ] Resource count nel log di `register_resources()` è 15
- [ ] Tutti i file in `package-manifest.json` di master esistono nel repo
- [ ] Tutti gli agenti master hanno frontmatter con `spark: true` e `layer: master`
- [ ] Tutti gli agenti dispatcher hanno `role: dispatcher`, `delegates_to_capabilities`, `fallback`
- [ ] `Agent-Research` ha `visibility: internal`
- [ ] `Agent-Orchestrator` ha `execution_mode`, `confidence_threshold`, `checkpoints`
- [ ] `.github/runtime/orchestrator-state.json` esiste con valori default
- [ ] `scf-pycode-crafter` non contiene più i file migrati al master
- [ ] `AGENTS-python.md` esiste in `scf-pycode-crafter`
- [ ] Tutti gli agenti `py-Agent-*` hanno `plugin`, `capabilities`, `languages` nel frontmatter
- [ ] `registry.json` contiene `scf-master-codecrafter` e `scf-pycode-crafter` v2.0.0
- [ ] CHANGELOG aggiornato in tutti e tre i repo
- [ ] NON esiste `py-Agent-Orchestrator.md` in `scf-pycode-crafter` (Opzione A)
