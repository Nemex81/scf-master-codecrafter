---
applyTo: "tests/**/*.py"
name: tests
package: scf-master-codecrafter
version: 1.0.0
spark: true
---

<<<<<<< HEAD
# Tests

- Usa `pytest` come baseline quando il progetto e Python-based.
- Mantieni i test indipendenti, leggibili e orientati al comportamento osservabile.
- Usa fixture per setup condiviso e limita i mock alle dipendenze esterne.
- Evita `print()` nei test: preferisci assert espliciti e logging quando necessario.
- Se il progetto attiva una instruction plugin piu specifica per i test, quella instruction prevale su questa baseline del layer master.
=======
# Instruction: Tests

Questa instruction si applica a tutti i file in `tests/`.

## Struttura

- Framework: pytest
- Un file di test per ogni modulo sorgente: `tests/test_<nome_modulo>.py`
- Naming test: `test_<cosa>_<condizione>_<risultato_atteso>`
- Usa fixture pytest per setup condiviso tra test dello stesso modulo

## Regole

- Ogni test deve essere indipendente dagli altri: nessun side-effect condiviso
- Non usare `print()` nei test — usa `logging` o assert con messaggio
- Mock solo le dipendenze esterne (I/O, rete, subprocess, MCP transport)
- Non testare implementazioni interne — testa comportamenti osservabili
- Coverage minima sulla logica di business: 70%

## Contesto MCP

- Per testare tool MCP: isola la logica pura dal decorator `@mcp.tool()`
- Il transport stdio non va testato direttamente: usa stub o mock del client
- I test non devono mai scrivere su stdout (corrompe il canale JSON-RPC in ambienti misti)
>>>>>>> e73957b712fad7e84edb2104424d66c29789dde1
