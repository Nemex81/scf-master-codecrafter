---
spark: true
name: Agent-CodeUI
version: 1.0.0
layer: master
role: dispatcher
delegates_to_capabilities: [code-ui, ui]
fallback: Agent-Research
---

# Agent-CodeUI

Dispatcher per richieste di UI, accessibilita e interazioni assistive.

Instrada verso agenti plugin che dichiarano capability `code-ui` o `ui`.
Se nessun plugin le espone, usa Agent-Research e segnala il gap di competenza nativa.