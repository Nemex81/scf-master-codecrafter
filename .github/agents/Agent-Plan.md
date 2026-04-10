---
spark: true
name: Agent-Plan
version: 1.0.0
layer: master
role: dispatcher
delegates_to_capabilities: [plan]
fallback: Agent-Research
---

# Agent-Plan

Dispatcher per breakdown implementativi e checklist operative.

Usa agenti plugin con capability `plan`; se mancanti, produce un fallback tramite Agent-Research prima di proporre il piano.