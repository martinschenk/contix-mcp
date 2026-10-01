# Installing the contix MCP servers

Both servers are remote (Streamable HTTP). Nothing to clone, build or run.

## contix herramientas (no auth)

Add this to the MCP settings:

```json
{
  "mcpServers": {
    "contix-herramientas": {
      "type": "streamableHttp",
      "url": "https://api.contix.es/api/v1/mcp/herramientas"
    }
  }
}
```

No API key, no account. Check: call `validate_iban` with `{"iban": "ES9121000418450200051332"}`; the answer has `"valido": true` and the bank `CAIXABANK, S.A.`.

## contix (invoicing, needs an account)

```json
{
  "mcpServers": {
    "contix": {
      "type": "streamableHttp",
      "url": "https://api.contix.es/api/v1/mcp",
      "headers": { "Authorization": "Bearer cx_live_YOUR_KEY" }
    }
  }
}
```

The user creates the key in the contix panel (Ajustes → API). Clients with OAuth support can omit the header and sign in instead.
