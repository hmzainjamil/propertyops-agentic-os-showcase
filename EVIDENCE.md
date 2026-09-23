# Evidence and measurements

The public showcase reports only measurements documented in the private engineering scorecard and evaluation artifacts.

## Verification summary

| Measure | Recorded result | Important limitation |
|---|---:|---|
| Automated tests | 396 | Synthetic and controlled test environment |
| Statement coverage | 90.7% | `propertyops/` package coverage |
| Release gates | 13/13 | Portfolio gate, not production certification |
| Emergency recall | 0.911 | Frozen out-of-sample rules test |
| Emergency safety | 239/239 | Generated + hand-written test messages avoided the routine reply |
| Reply intent accuracy | 0.917 | 48 hand-written examples in the documented Ollama evaluation |
| Load | 334.7 req/s | Real uvicorn, 600 requests, concurrency 50 |
| ACK p95 | 410 ms | Same controlled load run |
| Evidence register | 18 claims | Each claim mapped to evidence in the private project |

## What the numbers do not mean

These figures do not establish:

- production uptime
- real-provider reliability
- legal compliance
- enterprise security certification
- performance under real customer traffic
- accuracy on human-labelled production data
- economic ROI

## Evaluation discipline

The project deliberately separates development data from the frozen test set for the main emergency evaluation. The audit also checks whether headline claims can actually be reproduced from the code and tests rather than accepted from prose alone.
