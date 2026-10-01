<img src="assets/logo-400.png" width="72" alt="contix">

# contix MCP servers

Remote MCP servers by [contix](https://contix.es), Spanish invoicing software with Veri\*factu. Both are hosted; there is nothing to install. This repository holds only the public metadata (the server code is not open source).

## 1. contix herramientas (free, no account, no auth)

```
https://api.contix.es/api/v1/mcp/herramientas
```

Streamable HTTP, no authentication. Four read-only tools:

| Tool | What it does |
|---|---|
| `validate_iban` | IBAN check: country length and ISO 13616 check digits; for Spanish IBANs also the account check digits (DC) and the bank with its BIC (Banco de España registry). A 20-digit Spanish account number is converted to its IBAN. |
| `check_vat_number` | Looks up an EU VAT number in the European Commission's VIES registry and returns what the registry says. |
| `calculate_modelo_303` | Sums the boxes of the Spanish quarterly VAT return (Modelo 303) with the formulas of the official AEAT form. |
| `get_obligation_dates` | Veri\*factu and mandatory B2B e-invoicing dates in Spain from the published rules. |

Results are arithmetic or registry facts, never tax advice. The same tools are a plain REST API without key: `https://api.contix.es/api/v1/herramientas` ([docs](https://contix.es/api/#herramientas)). In the browser: [contix.es/herramientas](https://contix.es/herramientas/).

## 2. contix (invoicing, needs a contix account)

```
https://api.contix.es/api/v1/mcp
```

Streamable HTTP, OAuth 2.1 (dynamic client registration + PKCE) or API key. About 60 tools to run Spanish invoicing from your AI assistant: customers, draft invoices, finalizing into the Veri\*factu hash chain (asks for explicit confirmation), payments, expenses, recurring invoices, quotes, Facturae/FACe and Modelo 303/349/390 figures. Read-only and write scopes.

Setup for Claude, ChatGPT and Cursor: [contix.es/mcp](https://contix.es/mcp/) · API reference: [contix.es/api](https://contix.es/api/) · OpenAPI: `https://api.contix.es/openapi.json`

## Configuration

Clients that take a JSON config:

```json
{
  "mcpServers": {
    "contix-herramientas": { "type": "http", "url": "https://api.contix.es/api/v1/mcp/herramientas" },
    "contix": { "type": "http", "url": "https://api.contix.es/api/v1/mcp" }
  }
}
```

## Registry

Both servers are in the [Official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=contix) as `es.contix/herramientas` and `es.contix/contix` (`server.json` files here are copies).

## Contact

martin@contix.es · [Privacy](https://contix.es/legal/privacidad/) · [Terms](https://contix.es/legal/terminos/)

contix is software that issues invoices, not a tax advisory service.
