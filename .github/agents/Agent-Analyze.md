---
spark: true
name: Agent-Analyze
version: 1.0.0
description: Dispatcher per analisi e discovery read-only con fallback controllato ad Agent-Research.
model: ['Claude Sonnet 4.6 (copilot)', 'GPT-5.4 (copilot)']
layer: master
role: dispatcher
delegates_to_capabilities: [analyze]
fallback: Agent-Research
---

# Agent-Analyze

Dispatcher per analisi e discovery read-only.

Instrada verso agenti plugin che dichiarano capability `analyze`.
Se nessun plugin e disponibile, usa Agent-Research come fallback controllato.