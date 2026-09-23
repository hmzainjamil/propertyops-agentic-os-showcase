# Evidence and Measurements

This document is a public summary of engineering evidence recorded in the private implementation and scorecard.

## Core verification

| Metric | Result | What it means |
|---|---:|---|
| Automated tests | **396** | Broad unit, property, API, workflow, fault and regression coverage |
| Statement coverage | **90.7%** | Coverage of the propertyops package |
| Release gates | **13/13** | All current portfolio release gates passed |
| Emergency recall | **0.911** | Frozen out-of-sample deterministic rules test |
| Emergency false-emergency rate | **0.114** | Same frozen test |
| Emergency safety | **239/239** | Tested emergency messages avoided the routine reply |
| Reply intent accuracy | **0.917** | Documented hand-written evaluation using local Ollama |
| Deterministic reply decisions | **62.5%** | Hand-written evaluation decisions completed without a model call |
| Hybrid retrieval recall@3 | **0.955** | 26 labelled queries |
| Lead qualification | **1.0** | 100-example held-out evaluation |
| Intent-band classification | **1.0** | 100-example held-out evaluation |
| Load | **334.7 req/s** | Real uvicorn, 600 requests, concurrency 50 |
| ACK p95 | **410 ms** | Same load run |
| Chaos soak | **300 events** | 250 processed, 0 invariant violations in documented run |

## Engineering quality checks

| Check | Result |
|---|---|
| mypy | 0 errors |
| ruff | clean |
| bandit | 0 medium/high findings |
| pip-audit | 0 high/critical engine findings in current check |
| SBOM | 104 components |

## AI and retrieval evidence

The model layer is measured as a bounded part of the system, not as the entire system.

Documented results include:

- Local Ollama qwen2.5:7b evaluation on hand-written reply examples
- Opt-out recall 1.0 with false-positive rate 0.0 in the documented evaluation
- Emergency rules evaluated on a frozen out-of-sample set
- Hybrid retrieval evaluated against labelled queries
- Synthetic shadow lead-conversion model with AUC around 0.92 and Brier score around 0.10

The shadow ML model is not used to autonomously send, suppress or disqualify leads.

## Reliability evidence

The private project includes tests for:

- duplicate and replay handling
- partial failure and reconciliation
- dead-letter processing
- circuit-breaker paths
- approval timeouts
- provider acknowledgement timeouts
- state-version conflicts
- emergency duplicate suppression
- webhook validation
- fault injection and chaos behaviour

## Audit evidence

The hard audit deliberately found defects before remediation. The resulting regression suite covers those failure modes rather than only validating happy paths.

See [AUDIT.md](AUDIT.md).

## Evidence boundaries

These measurements do **not** establish:

- production uptime
- real-provider reliability
- production traffic capacity
- legal compliance
- independent security certification
- accuracy on human-labelled production data
- economic ROI

Evaluation data is synthetic, generated, controlled, or small-sample where explicitly stated.

## Reproducibility

The private implementation contains the evaluation commands, test suites, scorecard generation, load test and evidence artifacts used to produce the reported measurements.

The public repository intentionally exposes the results and methodology at a high level without publishing the complete source implementation or private evaluation assets.
