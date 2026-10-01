<img src="assets/logo-400.png" width="72" alt="contix">

# contix MCP server: Spanish invoicing with Veri*factu

Run your Spanish invoicing from Claude, ChatGPT, Cursor or any MCP client: customers, invoices with the Veri\*factu hash chain and QR, payments, expenses, quotes, recurring invoices, Facturae/FACe for the public administration, Modelo 303/349/390 figures, and a workspace for your accountant (gestoría). Hosted by [contix](https://contix.es), nothing to install. This repository holds the public metadata; the server code is not open source.

```
https://api.contix.es/api/v1/mcp
```

Streamable HTTP · OAuth 2.1 (dynamic client registration + PKCE) or API key (`Authorization: Bearer cx_live_…`, read-only or read/write) · needs a contix account (30-day trial). Irreversible steps, like finalizing an invoice into the Veri\*factu chain, ask for explicit confirmation.

Setup for Claude, ChatGPT and Cursor: [contix.es/mcp](https://contix.es/mcp/) · API reference: [contix.es/api](https://contix.es/api/) · OpenAPI: `https://api.contix.es/openapi.json`

## All 63 tools

**Invoices**

- `create_invoice`: Create draft invoice
- `update_invoice`: Update draft invoice
- `get_invoice`: Get invoice details · *read-only*
- `list_invoices`: List invoices · *read-only*
- `finalize_invoice`: Finalize invoice (irreversible) · *asks for confirmation / irreversible*
- `clone_invoice`: Clone invoice to draft
- `rectify_invoice`: Rectify invoice (Veri*factu)
- `cancel_invoice`: Cancel/annul invoice (irreversible) · *asks for confirmation / irreversible*
- `delete_invoice`: Delete draft invoice
- `send_invoice`: Send invoice by email

**Customers and payments**

- `list_customers`: List customers · *read-only*
- `get_customer`: Get customer · *read-only*
- `create_customer`: Create customer
- `update_customer`: Update customer
- `record_payment`: Record a payment
- `list_payments`: List payments of an invoice · *read-only*
- `get_account_statement`: Customer account statement · *read-only*
- `send_reminder`: Send payment reminder
- `list_reminders_sent`: List reminders sent · *read-only*
- `check_vat_number`: Check an EU VAT number (VIES) · *read-only*

**Quotes and recurring invoices**

- `list_quotes`: List quotes · *read-only*
- `create_quote`: Create quote
- `send_quote`: Send a quote by email
- `convert_quote_to_invoice`: Convert quote to invoice
- `list_recurring`: List recurring profiles · *read-only*
- `create_recurring`: Create recurring profile
- `generate_recurring`: Generate invoice from recurring profile

**Expenses and suppliers**

- `list_expenses`: List expenses · *read-only*
- `create_expense`: Create expense
- `update_expense`: Edit expense
- `delete_expense`: Delete expense
- `list_suppliers`: List suppliers · *read-only*

**Tax figures and public administration**

- `get_modelo`: Get AEAT tax model (modelo) · *read-only*
- `check_period`: Check a period before handing it over · *read-only*
- `get_invoice_facturae`: Get Facturae XML (B2G) · *read-only*
- `submit_invoice_to_face`: Submit invoice to FACe (B2G)
- `get_face_status`: Get FACe status (B2G) · *read-only*
- `calculate_modelo_303`: Sum the Spanish VAT return (Modelo 303) · *read-only*
- `get_obligation_dates`: Veri*factu and e-invoicing deadlines (Spain) · *read-only*
- `validate_iban`: Validate an IBAN · *read-only*

**Settings**

- `get_invoice_series`: Get invoice series/numbering · *read-only*
- `set_invoice_series`: Set invoice series/numbering
- `get_invoice_defaults`: Get invoice defaults · *read-only*
- `set_invoice_defaults`: Set invoice defaults
- `get_invoice_footer`: Get invoice footer · *read-only*
- `set_invoice_footer`: Set invoice footer
- `get_product_groups`: Get product groups · *read-only*
- `set_product_groups`: Set product groups
- `get_subscription_status`: Get subscription/trial status · *read-only*

**For accountants (gestorías) and their clients**

- `invite_gestoria`: Invite your gestoría
- `gestoria_listar_mandantes`: List gestoría clients · *read-only*
- `gestoria_invitar_mandante`: Invite a gestoría client
- `gestoria_abrir_mandante`: Open a gestoría client
- `gestoria_resumen_mandante`: Gestoría client: figures for a period · *read-only*
- `gestoria_facturas_mandante`: Gestoría client: list invoices · *read-only*
- `gestoria_gastos_mandante`: Gestoría client: list expenses · *read-only*
- `gestoria_revision_mandante`: Gestoría client: what is missing · *read-only*
- `gestoria_export`: Export all gestoría clients · *read-only*
- `gestoria_certificado_mandante`: Compliance certificate for a gestoría client · *read-only*
- `gestoria_comisiones`: Gestoría earnings from contix · *read-only*
- `gestoria_cierre_config`: Quarter-close reminders: current settings · *read-only*
- `gestoria_cierre_configurar`: Quarter-close reminders: change settings
- `gestoria_cierre_vista_previa`: Quarter-close reminder: preview for one client · *read-only*

The MCP server, the REST API and the web panel run on the same engine. The live list is `tools/list` on the endpoint.

## Free tools without an account

A second, smaller server with four read-only tools, no account and no auth:

```
https://api.contix.es/api/v1/mcp/herramientas
```

- `validate_iban`: IBAN check (country length, ISO 13616 check digits); for Spanish IBANs also the account check digits and the bank with its BIC from the Banco de España registry. A 20-digit Spanish account number is converted to its IBAN.
- `check_vat_number`: EU VAT number lookup in the European Commission's VIES registry.
- `calculate_modelo_303`: sums the boxes of the Spanish quarterly VAT return with the formulas of the official AEAT form.
- `get_obligation_dates`: Veri\*factu and mandatory B2B e-invoicing dates from the published rules.

Same tools as a REST API without key: `https://api.contix.es/api/v1/herramientas` ([docs](https://contix.es/api/#herramientas)). In the browser: [contix.es/herramientas](https://contix.es/herramientas/).

## Configuration

```json
{
  "mcpServers": {
    "contix": { "type": "http", "url": "https://api.contix.es/api/v1/mcp" },
    "contix-herramientas": { "type": "http", "url": "https://api.contix.es/api/v1/mcp/herramientas" }
  }
}
```

Clients without OAuth: add `"headers": { "Authorization": "Bearer cx_live_YOUR_KEY" }` to the `contix` entry; the key is created in the contix panel under Ajustes → API keys.

## Registry

Both servers are in the [Official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=contix) as `es.contix/contix` and `es.contix/herramientas` (the `server.json` files here are copies).

## Contact

martin@contix.es · [Privacy](https://contix.es/legal/privacidad/) · [Terms](https://contix.es/legal/terminos/)

contix is software that issues invoices, not a tax advisory service. Results are what you enter, computed; what is correct for your case you decide with your gestoría.
