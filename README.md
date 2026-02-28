# ecad-to-procurement-ai
AI-native procurement engine that turns BOMs into priced, purchased, and shipped components.

- Merchant of Record architecture  
- Inventory routes through the fulfillment node  
- Inspection and consolidation required  
- Deterministic state tracking  
- Inbound and outbound shipment legs tracked separately  

There is no drop-ship path in this protocol.

## Why This Exists

Hardware procurement is still manual:

- Cleaning messy BOM spreadsheets  
- Normalizing MPNs  
- Comparing distributor pricing  
- Issuing purchase orders  
- Tracking fragmented shipments  
- Reconciling packing slips  

AI can reason about sourcing.

Very few systems can execute physical commerce.

This protocol defines how AI agents can move from structured design output to shipped components.

## System Architecture

### 1. Digital Execution Layer

Responsible for:

- BOM ingestion (CSV, XLSX, Google Sheets)
- Column normalization
- MPN validation
- Inventory lookup
- Pricing comparison
- Optimization logic
- Quote generation
- Payment authorization (tokenized)
- Distributor PO issuance
- Deterministic order state transitions
- Immutable audit logging

Judgment tasks (alternate approval, lifecycle risk) are surfaced.

Mechanical tasks are automated end-to-end.

### 2. Physical Fulfillment Layer

After payment authorization:

1. Distributor POs are issued  
2. Distributors ship inventory to the fulfillment node  
3. Packing slip is matched to the PO  
4. MPN and quantity are verified  
5. Inspection is logged  
6. Multi-distributor shipments are consolidated  
7. Outbound shipment is created  
8. Tracking is synced to the order state  

Every order passes through this node.

## Deterministic Order State Machine

States:

- `draft`  
- `quoted`  
- `awaiting_confirmation`  
- `payment_authorized`  
- `po_issued`  
- `inbound_in_transit`  
- `inbound_received`  
- `inspected`  
- `consolidated`  
- `shipped`  
- `completed`  
- `exception`  

### Rules

- All state transitions emit immutable events  
- Write endpoints require idempotency keys  
- `shipped` refers to outbound shipment to the end user  
- Inbound and outbound tracking must be tracked independently  

See `docs/state-machine.md` for full details.

## Spreadsheet-Native Integration

Procurement teams operate in spreadsheets.

This protocol supports:

- Import BOM from Google Sheets or Excel  
- Write pricing results back into the sheet  
- Write distributor selection per line item  
- Trigger checkout from the sheet  
- Sync order status automatically  
- Sync inbound and outbound tracking numbers  

The spreadsheet remains the control interface.

The AI engine executes the workflow.

## Cross-Model Agent Compatibility

This protocol is model-agnostic.

Adapters included for:

- OpenAI (ChatGPT tool calling)
- Claude (Anthropic tools)
- Gemini (function calling)
- Grok (tool schema)

All models call the same backend.

No business logic lives in prompts.

uploadBom owns execution.

## Design Properties

- Schema-first tool calling  
- Idempotent transaction execution  
- Retry-safe orchestration  
- Merchant-of-record architecture  
- Tokenized payments only  
- Persistent order memory  
- Full audit trail   

## What This Is Not

- Marketplace directory  
- Affiliate sourcing engine  
- Speculative inventory warehouse  
- Front-capital purchasing system  

User pays first.  
Then POs are placed.

## Production Implementation

This repository defines the open protocol.

A production implementation of this model is available at:

https://uploadbom.com

---

## License

Apache License 2.0
