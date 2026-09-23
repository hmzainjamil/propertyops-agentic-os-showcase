# Capability Map

PropertyOps Agentic OS is broader than a single maintenance chatbot. The private implementation covers a lead-and-maintenance operating layer with explicit state, model routing, reliability controls and human governance.

## 1. Lead and CRM workflows

| Capability | Status | Scope |
|---|---|---|
| Lead webhook intake | Built | Signed intake, validation, idempotency, ACK-then-work |
| Lead qualification | Built | ICP scoring and intent bands |
| Outreach opener | Built | Template baseline plus bounded model drafting |
| Reply classification | Built | Intent, opt-out, objection and escalation handling |
| Stalled lead scanner | Built | Scheduled state inspection and reactivation |
| Meeting workflow | Built | Meeting offered and booked states |
| CRM synchronization | Adapter contract | Follow Up Boss behaviour mocked in portfolio environment |

Lifecycle states are modeled from NEW through qualification, outreach, engagement, meeting and conversion, with explicit exits for DISQUALIFIED, DO_NOT_CONTACT and FAILED.

## 2. Maintenance and property operations

| Capability | Status | Scope |
|---|---|---|
| Tenant message intake | Built | Structured tenant event handling |
| Emergency triage | Built | Deterministic rules first; unknown hazards fail safe |
| Work orders | Built | State, timeline, vendor attempts and reconciliation |
| Vendor dispatch | Adapter contract | ShowMojo / Rentvine / vendor behaviour mocked |
| Vendor acknowledgement | Built | Timer, retry and escalation path |
| Re-dispatch | Built | Idempotent provider-side action handling |
| Human takeover | Built | Timeout and unresolved-case escalation |
| Maintenance audit trail | Built | Decision and tool receipts |

## 3. Agent and model system

- Deterministic-first decision routing
- Local Ollama qwen2.5:7b classification
- Hosted fallback path for controlled synthetic evaluation
- Budgeted frontier escalation
- Structured outputs validated at the boundary
- Model confidence thresholds
- Prompt and model version control
- Model-call ledger with provider, model, tokens, cost and latency
- Hybrid retrieval using BM25 plus embedding search
- Tenant-filtered retrieval
- Shadow lead-conversion model trained only on synthetic data

## 4. Memory and state

Structured state is used instead of repeatedly prompting models with complete histories.

Tracked state includes:

- lifecycle stage
- recent events
- consent and suppression
- risk flags
- ICP and intent
- open work orders
- next action
- provider identifiers
- state version

Core records include events, leads, lead events, transitions, tool calls, model calls, approvals, DLQ items, work orders, traces and suppression records.

## 5. Reliability

- Idempotency keys
- Event status machine with reprocessing
- Compare-and-swap state versions
- Outbox processing
- Bounded retries with backoff and jitter
- Dead-letter queue and replay
- Business-level reconciliation
- Circuit breaker
- Approval timeouts
- Paging / fallback escalation
- Rate limiting
- Webhook ACK before background work
- Fault injection and chaos testing

## 6. Governance and compliance controls

Technical controls include:

- A0-A4 autonomy tiers
- Human approval for high-risk actions
- Immutable DecisionReceipt audit trail
- STOP / unsubscribe suppression
- Quiet-hours enforcement
- Frequency caps
- Sender identification
- Consent-aware messaging decisions
- Retention and erasure mechanisms
- PII redaction in traces
- Irreversible-action blocking

The compliance matrix is a technical self-assessment and explicitly requires legal review before production.

## 7. Observability and operator tooling

The Streamlit operator layer includes views for:

- overview
- leads
- replies
- maintenance
- approvals
- decisions
- evaluations
- failures
- health
- reliability
- traces
- analytics
- tenant setup

Operational instrumentation includes OpenTelemetry, Prometheus metrics, alert rules, DLQ visibility, model/tool ledgers and reconciliation reporting.

## 8. Integrations and orchestration

Provider adapter contracts exist for:

- Follow Up Boss
- Twilio
- ShowMojo
- Rentvine
- n8n

n8n workflow designs cover webhook intake, stalled-lead scanning and error handling.

## 9. Designed extension paths

The architecture has documented reuse paths for leasing, rent/delinquency, renewals, inspections, short-term-rental operations and owner communications.

These are **extension designs, not claims of completed real-provider production integrations**.

## 10. Explicitly out of scope

The portfolio build does not claim:

- real production provider credentials
- real customer data
- real outreach campaigns
- payment execution
- lease commitments
- legal decisions
- BricksFolios proprietary platform integration
- production Postgres / Redis deployment
- production alert routing
- independent security certification
