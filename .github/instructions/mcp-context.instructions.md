---
applyTo: "**"
name: mcp-context
package: scf-master-codecrafter
version: 1.0.0
spark: true
---

# MCP Context

- Quando il task tocca tool MCP, resource MCP, prompt framework o codice engine, preserva la separazione `stdout` vs `stderr`: nessun output applicativo su `stdout` fuori dal protocollo MCP.
- I tool pubblici devono essere registrati con il decorator corretto e mantenere docstring sintetiche e coerenti.
- Mantieni allineati contatori, changelog e documentazione pubblica quando il numero di tool o resource cambia.
- Per fix o review su contesto MCP, preferisci patch minime e non alterare il trasporto o la logica JSON-RPC senza richiesta esplicita.