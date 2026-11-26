# Security and privacy

**Status:** Required from phase one — implement as features land.  
Also see repo [`SECURITY.md`](../SECURITY.md).

## Access & credentials

- Strict tenant isolation + automated cross-tenant authorization tests
- Organization-based access control
- Scoped API keys; hashed API credentials; device-specific credentials
- Short-lived access tokens; refresh token rotation
- Secure password storage

## Data protection

- Encryption in transit and at rest
- Input validation; SQL injection prevention; rate limiting
- Audit logging; webhook authentication
- Account deletion; data export
- Consent records and revocation
- Configurable retention; automated data deletion

## Engineering hygiene

- Dependency, container vulnerability, and secret scanning

## Privacy product rules

- Clear explanation of collected data, purposes, and retention
- Support Canadian and international privacy needs **without claiming automatic legal compliance**
- **No covert tracking**
- **Do not log sensitive GPS coordinates in general application logs**
