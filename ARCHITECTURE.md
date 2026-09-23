# Architecture

## Design principles

### 1. Deterministic first

Rules, state machines and policy checks handle safety-critical and predictable decisions wherever possible.

### 2. Models are a bounded component

A model is not the source of truth for every workflow. Model use is constrained by routing, budgets, policy, confidence and escalation rules.

### 3. State is explicit

The workflow tracks tenant and operational state rather than relying on conversation history alone.

### 4. Side effects are controlled

Actions are passed through idempotency keys, an outbox pattern, retry handling and dead-letter processing.

### 5. Human escalation is a feature

A0-A4 autonomy tiers define what the system may do automatically and when a human must intervene.

## Main flow

```text
1. Receive event
2. Authenticate sender / webhook
3. Assign idempotency key
4. Load tenant and workflow state
5. Apply deterministic safety and policy rules
6. Retrieve relevant knowledge if required
7. Invoke local or stronger model only when needed
8. Produce structured decision
9. Apply autonomy tier
10. Execute or escalate
11. Persist audit receipt
12. Update state
13. Record telemetry
14. Retry, replay or dead-letter on failure
```

## Autonomy tiers

| Tier | Meaning |
|---|---|
| A0 | No autonomous action. Human decision required. |
| A1 | Low-risk assistive action with strong constraints. |
| A2 | Bounded workflow action with policy validation. |
| A3 | Higher autonomy within explicit guardrails and escalation rules. |
| A4 | Highest permitted autonomy under the configured governance policy. |

The tier is a control mechanism, not a statement that the system is safe for unrestricted deployment.

## Reliability controls

The implementation includes:

- idempotency and duplicate suppression
- outbox processing
- retry limits
- dead-letter queue and replay
- reconciliation after partial failure
- circuit-breaker behaviour
- approval timeouts
- fallback escalation
- provider adapter contracts

## Audit trail

Each material decision can produce an immutable DecisionReceipt containing enough structured context to reconstruct what the system decided, what policy path it used, what action followed, and whether a human decision was involved.
