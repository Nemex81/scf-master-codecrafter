---
applyTo: "**/*.py"
name: python
package: scf-master-codecrafter
version: 1.0.0
spark: true
---

# Instruction: Python

Questa instruction si applica a tutti i file `.py` del workspace.

## Standard

- Python 3.10+ con type hints obbligatori per funzioni pubbliche
- Docstring per moduli, classi pubbliche e funzioni pubbliche
- f-string per formattazione stringhe
- `pathlib.Path` per operazioni su file (no `os.path`)
- Dataclass frozen per strutture dati immutabili
- Context manager per risorse (file, connessioni, lock)

## Stile

- Nomi variabili e funzioni in `snake_case`
- Nomi classi in `PascalCase`
- Costanti in `UPPER_SNAKE_CASE`
- Lunghezza riga max 100 caratteri
- Import ordinati: stdlib, third-party, locale (isort/ruff)
- `from __future__ import annotations` in testa ad ogni modulo

## Errori

- Gestione esplicita con eccezioni tipizzate
- Non usare `except Exception` senza motivazione documentata
- Non sopprimere eccezioni con `pass`
- Log degli errori via `logging` prima di ri-sollevare
- Mai scrivere su stdout: usare `sys.stderr` o file di log (regola critica per server MCP stdio)

## Architettura

- Separazione netta tra logica e I/O
- Funzioni e classi con responsabilita singola e ben definita
- Nomi di variabili esplicativi: vietati `x`, `tmp`, `data` senza qualificatore
- Evitare soluzioni "magiche" non documentate: se c'e un trade-off, dichiararlo

## Test

- Framework: pytest
- File di test: `tests/test_<nome_modulo>.py`
- Naming: `test_<cosa>_<condizione>_<risultato_atteso>`
- Evita mock eccessivi: testa comportamenti, non implementazioni
