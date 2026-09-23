# Architecture

## System objective

The system is designed as an event-driven operating layer across CRM, messaging, maintenance and related property workflows.

The guiding sequence is:

~~~text
Event -> State -> Deterministic Logic -> Retrieval
     -> Model if Needed -> Action -> Validation
     -> State Update -> Next Action
~~~

## Architecture layers

### 1. Event gateway

Inbound provider events are:

- authenticated
- schema-validated
- assigned an idempotency key
- acknowledged quickly
- moved to background processing

The portfolio API includes webhook paths for general events and Twilio callbacks.

### 2. State layer

State is explicit and durable in the portfolio environment.

The model includes records for:

| Record | Purpose |
|---|---|
| Events | Idempotency, audit and processing status |
| Leads | Canonical lifecycle, ICP, intent and next action |
| Lead events | Compact interaction history |
| Transitions | Who changed state, why and with what confidence |
| Tool calls | Side effects, attempts and idempotency |
| Model calls | Tier, provider, model, tokens, cost and latency |
| Approvals | Human decisions and outcomes |
| DLQ | Failed events and tool calls |
| Work orders | Maintenance lifecycle and vendor timeline |
| Traces | Redacted execution detail |
| Suppression | Opt-out and erasure tombstones |

State transitions use version checks to reduce concurrent-write races.

### 3. Deterministic policy layer

Rules handle decisions that do not need probabilistic reasoning, including:

- authentication
- idempotency
- opt-out and suppression
- quiet hours
- frequency caps
- routing
- state transitions
- hazard detection
- irreversible-action blocking

### 4. Retrieval layer

Knowledge retrieval is tenant-filtered and uses a hybrid approach combining lexical and embedding retrieval. The goal is to supply only the records and knowledge needed for the current decision.

### 5. Model router

The router uses progressively more expensive reasoning:

~~~text
Rules
  |
  +--> no model required
  |
  +--> local Ollama classification
          |
          +--> accepted
          |
          +--> fallback / stronger reasoning
                    |
                    +--> frontier model if budget and policy allow
                    |
                    +--> human escalation
~~~

Model calls are schema-constrained, confidence-checked and ledgered.

### 6. Action layer

Side effects go through bounded tools and provider adapters.

Key controls include:

- idempotency keys
- retry limits
- outbox processing
- timeout handling
- provider acknowledgement
- dead-letter routing
- reconciliation
- human takeover

### 7. Governance layer

A0-A4 autonomy tiers define what the system may do automatically.

Material decisions can produce a DecisionReceipt describing the decision path, policy context, actor, confidence and resulting action.

### 8. Observability layer

The portfolio implementation includes:

- OpenTelemetry spans
- redacted trace records
- model and tool ledgers
- Prometheus-format metrics
- health and reliability views
- alert-rule evaluation
- reconciliation reporting

## Workflow examples

### Lead workflow

~~~text
lead.created
 -> validate + dedupe
 -> load lead state
 -> ICP / intent scoring
 -> policy check
 -> outreach or human review
 -> record action
 -> wait for next event
~~~

### Maintenance workflow

~~~text
tenant.message
 -> authenticate + identify tenant
 -> load property context
 -> deterministic hazard check
 -> local model only if needed
 -> severity / policy decision
 -> create or update work order
 -> vendor dispatch
 -> acknowledgement timer
 -> retry / re-dispatch / human takeover
 -> reconcile state
~~~

### Reply workflow

~~~text
lead.reply
 -> suppression / opt-out check
 -> deterministic classification where possible
 -> local model for ambiguous intent
 -> escalation for objection / high-value / uncertainty
 -> validated response or human review
 -> CRM state update
~~~

## Provider boundary

The architecture defines adapter contracts for:

- Follow Up Boss
- Twilio
- ShowMojo
- Rentvine
- n8n

The portfolio environment uses mocked provider behaviour. Real sandbox validation is a remaining external dependency.

## n8n boundary

n8n is used as an orchestration boundary rather than the source of truth for core state.

Current design workflows cover webhook intake, stalled-lead scanning and error handling.

Core policies, state transitions, idempotency and safety remain in application code.

## Failure model

The system assumes events and provider calls can fail, repeat or complete partially.

Recovery mechanisms include:

- reprocessable event status
- bounded retry with backoff
- DLQ + replay
- compare-and-swap state versions
- reconciliation
- circuit breaker
- approval timeout escalation
- fallback human routing

The project was deliberately audited against these failure modes.

## Deployment boundary

The current portfolio environment uses SQLite and Docker-friendly packaging.

Production-shaped elements such as Postgres / Redis, real provider sandboxes, deployed telemetry collection and real alert routing are documented as next-stage infrastructure rather than claimed production capabilities.
