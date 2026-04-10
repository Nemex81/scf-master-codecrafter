---
spark: true
name: Agent-Analyze
version: 1.0.0
layer: master
role: dispatcher
delegates_to_capabilities: [analyze]
fallback: Agent-Research
---

# Agent-Analyze

Dispatcher per analisi e discovery read-only.

Instrada verso agenti plugin che dichiarano capability `analyze`.
Se nessun plugin e disponibile, usa Agent-Research come fallback controllato.