---
spark: true
name: Agent-Design
version: 1.0.0
layer: master
role: dispatcher
delegates_to_capabilities: [design]
fallback: Agent-Research
---

# Agent-Design

Dispatcher per decisioni architetturali e documenti di design.

Usa agenti plugin con capability `design`; in assenza di copertura, richiede ad Agent-Research un brief architetturale preliminare.