---
name: spanish-invoicing
description: Use when someone in Spain wants to create, send, correct or look up invoices, quotes, payments or expenses in their contix account, needs Veri*factu or Facturae/FACe for an invoice, wants the figures of their Modelo 303/349/390, or just needs to check an IBAN, an EU VAT number or a filing date without an account.
---

# Spanish invoicing with contix

contix is invoicing software. It creates invoices from the user's own data and
computes figures; it does not give tax advice. What is correct for taxes the user
decides with their gestoría or tax adviser. Never suggest which rate, value or
form someone should use.

1. **No account needed:** `contix-herramientas` checks an IBAN (`validate_iban`),
   an EU VAT number in VIES (`check_vat_number`), computes Modelo 303 figures from
   amounts the user gives (`calculate_modelo_303`) and lists AEAT filing dates
   (`get_obligation_dates`).
2. **Own account:** the `contix` server signs in with OAuth (or an API key from the
   contix panel). Typical flow: find or create the customer, `create_invoice` as a
   draft, show it, then `finalize_invoice`.
3. **Irreversible steps ask first:** finalizing puts the invoice into the Veri*factu
   chain and it can no longer be edited; corrections go through `rectify_invoice`,
   cancellations through `cancel_invoice`. Always confirm with the user before these.
4. Sending by email, payment reminders, Facturae/FACe for public bodies and the
   accountant workspace work through their own tools; read-only tools are marked.
