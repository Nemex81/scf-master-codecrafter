{
  "schema_version": "2.0",
  "package": "scf-pycode-crafter",
  "version": "2.0.0",
  "display_name": "SCF Python Code Crafter",
  "description": "Agenti, skill e istruzioni specializzati per lo sviluppo Python. Richiede scf-master-codecrafter.",
  "author": "Nemex81",
    "min_engine_version": "1.9.0",
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
    "engine_min_version": "1.9.0",
  "tags": ["master", "orchestrator", "dispatcher", "base"]
}
```

Aggiornare entry esistente `scf-pycode-crafter`:
- `latest_version`: `"1.2.1"` → `"2.0.0"`
- Aggiungere `"engine_min_version": "1.9.0"`

---

## Checklist Finale per Copilot
