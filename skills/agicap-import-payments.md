---
name: Manage beneficiaries and import payment files
description: Register payment beneficiaries and import payment/remittance files for execution in Agicap.
api: openapi/agicap-payments-v2-openapi.json
operations: [2_Beneficiaries_Add, 1_Beneficiaries_Get_All, 6_Beneficiaries_Sync_Start, 7_Beneficiaries_Sync_Report, PaymentsFile_Import_Post, PaymentsFile_SecuredImport_Post]
---

# Manage beneficiaries and import payment files

Register the beneficiaries you pay, then import payment or remittance files for processing.

## Steps

1. **Authenticate** with scope `agicap:public-api` plus the relevant payment scope
   (`public-api:manage-payment-beneficiaries`, `public-api:import_payment_files`, or
   `public-api:import_payment_files_with_signed_beneficiaries`).
2. **Add beneficiaries** individually with `2_Beneficiaries_Add`
   (`POST /public/payments/v2/entities/{entityId}/Beneficiaries`) or in bulk with
   `6_Beneficiaries_Sync_Start`, polling `7_Beneficiaries_Sync_Report` for the outcome.
3. **List** current beneficiaries with `1_Beneficiaries_Get_All`; edit with `3_Beneficiaries_edit`.
4. **Import a payment file** with `PaymentsFile_Import_Post`, or use
   `PaymentsFile_SecuredImport_Post` when signed-beneficiary controls are required.

## Rules
- Payment operations are high-consequence — obtain the minimal fine-grained scope for each action (see `scopes/agicap-scopes.yml` and `agentic-access/agicap-agentic-access.yml`).
- All calls are entity-scoped via the `{entityId}` path parameter.
- Errors are `application/problem+json` (RFC 9457); a `409` indicates a state conflict.
