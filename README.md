# SCF Master CodeCrafter

Plugin CORE-CRAFT del SPARK Code Framework.

## Cos'e

`scf-master-codecrafter` non e piu il layer base del framework. Il layer fondazionale e `spark-base`.
Questo pacchetto aggiunge sopra `spark-base` i componenti trasversali di design e code routing:

- `code-Agent-Code`
- `code-Agent-Design`
- `code-Agent-CodeRouter`
- `code-Agent-CodeUI`
- `mcp-context.instructions.md`
- skill `clean-architecture`, `code-routing`, `docs-manager`
- `AGENTS-master.md`, changelog e sezione condivisa di `copilot-instructions.md`

## Perimetro attuale

Il manifest corrente del pacchetto e `package-manifest.json` schema `3.1`, versione `2.7.0`, con 19 file gestiti:

- 11 agenti
- 1 instruction
- 3 skill
- 3 file di configurazione condivisi

## Dipendenze

Questo pacchetto richiede:

- `spark-base`
- `spark-framework-engine >= 3.6.0`

I plugin linguaggio-specifici, come `scf-pycode-crafter`, dipendono da questo layer ridotto.

## Installazione

```python
scf_install_package("scf-master-codecrafter")
```

Il motore deve trovare prima `spark-base` installato, oppure risolvere la dipendenza dichiarata nel manifest.

## Plugin compatibili

| Package | Versione | Linguaggio |
| --- | --- | --- |
| `scf-pycode-crafter` | `2.3.0` | Python |

## Maintainer

Nemex81
