# Security Policy

Aimlake Open Telematics is privacy-first and security-first from the first development phase.

## Reporting

Report vulnerabilities privately to **mail@aimlake.com** with subject `Aimlake Open Telematics security`. Do not open public issues for undisclosed vulnerabilities.

## Non-negotiables

- Strict tenant isolation and automated cross-tenant authorization tests
- Scoped, hashed API credentials; short-lived tokens; refresh rotation
- Encryption in transit and at rest
- No covert tracking
- Do not log sensitive GPS coordinates in general application logs
- No installation analytics without explicit opt-in
- Self-hosted core: no remote licensing, forced cloud, or feature locks

Details: [`docs/SECURITY_PRIVACY.md`](docs/SECURITY_PRIVACY.md) · full plan: [`docs/PLAN.md`](docs/PLAN.md)
