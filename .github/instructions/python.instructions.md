---
applyTo: "**/*.py"
name: python
package: scf-master-codecrafter
version: 1.0.0
spark: true
---

# Python

- Applica type hints a funzioni pubbliche e dati strutturati quando il contesto lo consente.
- Preferisci `pathlib.Path`, `dataclass` e `f-string` al posto di pattern legacy equivalenti.
- Mantieni nomi in `snake_case` per funzioni e variabili, `PascalCase` per classi, `UPPER_SNAKE_CASE` per costanti.
- Evita refactor non richiesti e mantieni le modifiche locali e minimali.
- Se esiste una instruction plugin piu specifica per Python, considerala prioritaria rispetto a questa baseline del layer master.