# Aimlake Open Telematics

**Open technology. Intelligent mobility.**  
Mobility intelligence. Without limits.

Production-oriented, open-source smartphone telematics infrastructure by **Aimlake Inc.**  
Apache 2.0 · Free forever self-hosted · Optional managed cloud and professional services.

Lives under the **Aimlake Developer** ecosystem: https://developer.aimlake.com

## Mission

Independent alternative to proprietary smartphone telematics—without mandatory expensive third-party telematics subscriptions. Optimize for developer adoption, tech leadership, OSS contributions, organic marketing, and enterprise opportunities—not API sales.

## Status

Plan locked; **build as needed**.

- [`docs/PLAN.md`](docs/PLAN.md) — full product plan
- [`docs/API.md`](docs/API.md) — REST `/v1` surface + webhooks
- [`docs/MCP.md`](docs/MCP.md) — MCP tools and auth defaults
- [`docs/CONSOLE.md`](docs/CONSOLE.md) — developer dashboard + API playground
- [`docs/SELF_HOSTING.md`](docs/SELF_HOSTING.md) — Apache 2.0 self-host guarantees
- [`docs/SECURITY_PRIVACY.md`](docs/SECURITY_PRIVACY.md) — security from phase one
- [`docs/TESTING.md`](docs/TESTING.md) — simulator + benchmarks
- [`docs/RELEASE_QUALITY.md`](docs/RELEASE_QUALITY.md) — OSS release checklist
- [`ROADMAP.md`](ROADMAP.md) — public roadmap

## Monorepo

| Path | Role |
|---|---|
| `apps/` | api, worker, dashboard, website, documentation, mcp-server |
| `packages/` | telemetry, trip, scoring, geospatial, event-detection, shared-types |
| `sdk/` | ios, android, react-native, flutter, javascript, python |
| `examples/` | delivery, fleet, personal driving, demo |
| `infrastructure/` | docker, aws, terraform |

Stack: FastAPI · PostgreSQL/PostGIS · Celery · Valkey · Next.js · MapLibre · Docker Compose · OpenAPI 3.1. Modular monolith first.

## Partners

Grabbey, Torqon, and AIVAS integrate externally. **Do not** hardcode their business logic into core.

## License

Apache License 2.0 — see [`LICENSE`](LICENSE).
