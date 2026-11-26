# Self-hosting

**Status:** Planned — build as needed.  
**License:** Apache 2.0 for the entire core platform.

## Guarantees

- Unlimited software-level device capacity and API requests (infra is the only limit)
- Configurable data retention
- Full core: dashboard, REST API, MCP, SDKs, webhooks, fleets, geofences, driver analytics

## Must not include

- Mandatory Aimlake authentication
- Remote licensing verification
- Forced cloud connectivity
- Feature locks
- Installation analytics without explicit opt-in

## Install (provisional repo URL)

```bash
git clone https://github.com/aimlake/telematics.git
cd telematics
cp .env.example .env
docker compose up -d
```

Update the clone URL when the real public repository is published.

## Also ship

Installation wizard · initial administrator setup · database migrations · secure secret generation · HTTPS deployment guidance · backup and restore · upgrades · ARM64 and AMD64 containers · environment configuration docs

## Independence

Usable without Amazon AWS, Google Maps, OpenAI, or other commercial services. Prefer MapLibre with configurable tile providers and local/S3-compatible storage.
