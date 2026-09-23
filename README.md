# PropertyOps Agentic OS

### Deterministic-first agent architecture for 24/7 residential property operations

PropertyOps Agentic OS is a hands-on engineering portfolio project for building a reliable agentic operating layer across CRM, messaging, leasing, maintenance and operational workflows.

> **Core design principle:** use ordinary software for decisions that can be deterministic. Use an LLM only when reasoning or classification is actually needed.

**Status:** Portfolio / controlled sandbox  
**Live system:** https://propertyops-agentic-os.streamlit.app/  
**Public showcase:** https://github.com/hmzainjamil/propertyops-agentic-os-showcase  
**Full implementation:** private source repository

> **Scope:** The public repository is documentation-only. It does not contain the complete implementation, credentials, production secrets, customer data or private evaluation assets.

---

## At a glance

| Area | Approach |
|---|---|
| Runtime | Python, FastAPI, Streamlit, Docker |
| State | Tenant / prospect state, versioned transitions, compare-and-swap |
| Data | SQLite in the portfolio environment |
| Models | Ollama qwen2.5:7b, routed fallbacks, budgeted frontier escalation |
| Retrieval | Hybrid BM25 + embedding retrieval with tenant filtering |
| Orchestration | Core workflow engine plus n8n workflow designs |
| Integrations | Follow Up Boss, Twilio, ShowMojo, Rentvine adapter contracts |
| Reliability | Idempotency, outbox, retries, DLQ, replay, reconciliation, circuit breaker |
| Governance | A0-A4 autonomy tiers, human approval, DecisionReceipt audit trail |
| Observability | OpenTelemetry, Prometheus metrics, traces and alert rules |
| Evaluation | Unit, property-based, API contract, fault, chaos, load and adversarial testing |

---

## Why the architecture is different

A basic automation often looks like:

~~~text
Trigger -> Send full history to GPT -> Response -> Next node
~~~

This project uses:

~~~text
Event -> State -> Deterministic logic -> Targeted retrieval
      -> Model only when needed -> Validated action
      -> State update -> Next action
~~~

The system keeps state and safety-critical decisions under explicit software control and uses models as bounded components.

---

## Capability coverage

The private implementation contains more than the public repository exposes.

| Capability | Status |
|---|---|
| Lead intake, qualification, outreach and reply handling | Built |
| Stalled-lead detection and reactivation | Built |
| Meeting workflow | Built |
| Maintenance intake and emergency triage | Built |
| Work orders, vendor dispatch and follow-through | Built |
| Human approval and escalation | Built |
| Tenant setup and tenant-scoped configuration | Built |
| Analytics, health and reliability views | Built |
| Decision and trace inspection | Built |
| Compliance and suppression controls | Built |
| Local model routing and hybrid retrieval | Built |
| Shadow ML lead scoring | Built |
| n8n workflows | Designed |
| Real provider E2E | Not yet |
| BricksFolios proprietary platform integration | Not yet |

See [CAPABILITIES.md](CAPABILITIES.md).

---

## Architecture

### Control loop

~~~text
Event
  -> Authenticate + validate
  -> Idempotency / event status
  -> Load structured state
  -> Deterministic rules
  -> Targeted retrieval
  -> Model only when needed
  -> Structured decision
  -> A0-A4 policy gate
  -> Human approval or validated action
  -> Outbox / retry / DLQ
  -> DecisionReceipt + telemetry
  -> State update
~~~

### Model routing

1. Deterministic rules get first decision rights.
2. Local Ollama handles eligible lightweight classification.
3. A controlled fallback path can be used when the local model is unavailable.
4. Frontier reasoning is budgeted and reserved for harder cases.
5. Invalid or low-confidence outputs escalate rather than silently producing unsafe actions.
6. Model calls record tier, provider, model, tokens, cost and latency.

---

## State, memory and context

The system does not repeatedly send complete customer histories to a model.

Structured state includes:

- lifecycle stage
- recent meaningful events
- consent and suppression
- open work orders
- risk flags
- ICP and intent
- next action
- provider identifiers
- state version

Retrieval is tenant-filtered and only relevant context is assembled for the current decision.

---

## Reliability and failure recovery

Implemented controls include:

- unique event and tool idempotency keys
- reprocessable event status
- compare-and-swap state versions
- bounded retries with backoff and jitter
- outbox processing
- dead-letter queue and replay
- reconciliation after partial failure
- circuit breaker
- approval timeouts
- fallback escalation
- webhook ACK before background work
- rate limiting
- provider-side duplicate accounting

See [ARCHITECTURE.md](ARCHITECTURE.md).

---

## Governance and safety

A0-A4 autonomy tiers define how much the system may act without a human.

Deterministic safety controls cover areas such as:

- opt-out and suppression
- quiet hours
- frequency caps
- sender verification
- webhook signatures
- high-risk maintenance routing
- irreversible-action blocking
- human approval on high-risk paths

The compliance matrix is a technical self-assessment, not legal sign-off.

---

## Integrations and orchestration

Provider adapter contracts cover:

- Follow Up Boss
- Twilio
- ShowMojo
- Rentvine
- n8n

n8n designs cover webhook intake, stalled-lead scanning and error handling.

The core workflow engine retains state, policy, idempotency and safety logic rather than moving business-critical decisions into n8n.

---

## Operator surfaces

The live Streamlit app includes views for leads, replies, maintenance, approvals, decisions, evaluations, failures, health, reliability, traces, analytics and tenant setup.

The goal is not only automation. It is inspectable automation.

---

## Measured evidence

| Metric | Result |
|---|---:|
| Automated tests | **396** |
| Statement coverage | **90.7%** |
| Release gates | **13/13** |
| Emergency recall | **0.911** |
| Emergency false-emergency rate | **0.114** |
| Emergency safety | **239/239** |
| Reply intent accuracy | **0.917** |
| Deterministic reply decisions | **62.5%** |
| Hybrid retrieval recall@3 | **0.955** |
| Lead qualification | **1.0** |
| Intent-band classification | **1.0** |
| Load | **334.7 req/s** |
| ACK p95 | **410 ms** |
| Chaos soak | **300 events, 0 invariant violations** |
| mypy | **0 errors** |
| ruff | **clean** |
| bandit | **clean** |
| SBOM | **104 components** |

These are controlled engineering results. They are not production guarantees.

See [EVIDENCE.md](EVIDENCE.md).

---

## Audit-driven development

The project was intentionally subjected to an adversarial hard audit.

Early findings included unsigned webhook acceptance, emergency-rule overfitting, lost leads after failure, opt-out defects, duplicate emergency dispatch, tenant identity spoofing, incomplete approval paths and simulator fail-open behaviour.

Those findings were converted into fixes and regression tests.

The key evidence is the failure-and-remediation cycle, not a claim that the first version was perfect.

See [AUDIT.md](AUDIT.md).

---

## Quality and security posture

The private project includes:

- boundary validation
- signed webhooks and Twilio signature verification
- rate limiting
- PII redaction
- consent and suppression controls
- retention and erasure mechanisms
- model and tool ledgers
- OpenTelemetry
- Prometheus metrics and alerts
- Docker packaging
- static analysis and dependency checks
- adversarial and fault-injection tests

No independent security certification or penetration-test sign-off is claimed.

---

## Repository structure

~~~text
.
├── README.md
├── CAPABILITIES.md
├── ARCHITECTURE.md
├── EVIDENCE.md
├── AUDIT.md
└── LIMITATIONS.md
~~~

The complete implementation, tests, datasets, model registry, deployment configuration and internal documentation remain private.

---

## Production boundary

This project is a serious engineering portfolio artifact and controlled sandbox design. It is **not** a production-certified property-management platform.

Before real production use it still needs:

1. Real sandbox testing with provider APIs
2. Independent penetration testing and security review
3. Human-labelled production-like evaluation data
4. Production Postgres / Redis deployment and recovery testing
5. Deployed telemetry collection and real alert routing
6. Jurisdiction-specific legal and compliance review
7. Integration with the target Node.js / MongoDB / RDS platform

See [LIMITATIONS.md](LIMITATIONS.md).

---

## Technical review

A technical review can cover:

- architecture and state transitions
- model routing and token efficiency
- retrieval and context assembly
- safety and autonomy controls
- idempotency and failure recovery
- evaluation methodology
- audit findings and remediation

### Live demo

https://propertyops-agentic-os.streamlit.app/

### Public showcase

https://github.com/hmzainjamil/propertyops-agentic-os-showcase

### Full source

The complete implementation remains private and can be made available for controlled technical evaluation.

---

## Scope statement

This public repository documents the architecture and engineering evidence of PropertyOps Agentic OS.

It intentionally does not expose the complete implementation.

It should be reviewed as an engineering artifact, not mistaken for production certification.
