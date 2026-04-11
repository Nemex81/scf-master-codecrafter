---
applyTo: "tests/**/*.py"
name: tests
package: scf-master-codecrafter
version: 1.0.0
spark: true
---

# Tests

- Usa `pytest` come baseline quando il progetto e Python-based.
- Mantieni i test indipendenti, leggibili e orientati al comportamento osservabile.
- Usa fixture per setup condiviso e limita i mock alle dipendenze esterne.
- Evita `print()` nei test: preferisci assert espliciti e logging quando necessario.
- Se il progetto attiva una instruction plugin piu specifica per i test, quella instruction prevale su questa baseline del layer master.