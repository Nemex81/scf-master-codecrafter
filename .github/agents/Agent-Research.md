---
spark: true
name: Agent-Research
version: 1.0.0
layer: master
role: executor
visibility: internal
output_path: .github/runtime/research-cache/
---

# Agent-Research

Fallback interno per ricerche su stack o framework non coperti dai plugin installati.

## Output atteso

- brief strutturato in `.github/runtime/research-cache/{language}-{task-type}.md`;
- limiti dichiarati esplicitamente;
- assunzioni e fonti di ricerca elencate in modo testuale.

## Regole

- L'output non sostituisce una competenza nativa di plugin.
- Segnala sempre che il brief e un fallback dinamico.