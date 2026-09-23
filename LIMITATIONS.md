# Limitations and production gaps

This project is an engineering portfolio artifact and controlled sandbox design. It is not presented as production-ready software.

## Remaining external validation

The internal scorecard identifies the following major gaps before real production use:

### Real provider validation

Provider integrations for Follow Up Boss, Twilio, ShowMojo and Rentvine use mocked adapters in the portfolio environment. Real sandbox credentials and provider contract tests are still required.

### Independent security review

The project includes security controls and automated checks, but it has not received an independent penetration test or external security assessment.

### Human-labelled evaluation

The evaluation datasets are synthetic or generated and filtered. A production deployment would need a substantial human-labelled test set maintained by domain staff.

### Production infrastructure

The portfolio system is not a substitute for a production Postgres/Redis topology, disaster recovery process, deployed telemetry collector, alert routing and operational SLOs.

### Compliance and legal review

The system contains technical controls related to opt-out, consent, quiet hours and other policy rules, but technical controls are not legal approval. Real deployment requires jurisdiction-specific legal review.

## Security boundary

The public repository intentionally excludes:

- source implementation
- secrets and credentials
- provider keys
- customer data
- internal deployment configuration
- any private evaluation material that could expose sensitive implementation details
