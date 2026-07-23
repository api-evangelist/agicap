---
name: Synchronize business documents into Agicap
description: Push client and supplier invoices into Agicap so they appear as expected transactions and receivables.
api: openapi/agicap-business-documents-v2-openapi.json
operations: [Organizations_ListEntities, Connection_CreateConnection, ClientInvoice_CreateClientInvoices, SupplierInvoice_CreateSupplierInvoicesV2, ClientInvoice_GetClientInvoicesV2]
---

# Synchronize business documents into Agicap

Push unpaid/partially-paid invoices and credit notes from your ERP into Agicap so they drive
cash-flow forecasts (Treasury) and receivables follow-up (Accounts Receivable).

## Steps

1. **Authenticate** (see `agicap-authenticate.md`) and pick the target entity with `Organizations_ListEntities`.
2. **Create a connection** for that entity with `Connection_CreateConnection`
   (`POST /public/business-documents/v2/entities/{entityid}/connections`). Reuse the returned `connectionid` for all documents.
   - New connections require one-time activation by Agicap support before documents flow.
3. **Create client invoices** with `ClientInvoice_CreateClientInvoices`
   (`POST .../connections/{connectionid}/client-invoices`) and **supplier invoices** with
   `SupplierInvoice_CreateSupplierInvoicesV2`.
4. **Verify** with `ClientInvoice_GetClientInvoicesV2`. Keep documents current by updating with
   `ClientInvoice_UpdateClientInvoices` when payment status changes.

## Rules
- Only synchronize unpaid or partially-paid documents that generate future expected transactions; historical paid documents are usually unnecessary.
- Sync at least once before 09:00 (when users log in) for fresh morning data.
- Use `If-Match`/`If-None-Match` conditional headers for optimistic concurrency; there is no Idempotency-Key.
- Errors are `application/problem+json` (RFC 9457).
