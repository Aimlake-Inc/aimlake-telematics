# Aimlake Open Telematics — Boss plan

Implement **as needed**. This document is the source of truth for product scope. Do not invent partner-specific (Grabbey / Torqon / AIVAS) business logic in core.

## 3. Repository structure

```text
aimlake-telematics/
├── apps/          api, worker, dashboard, website, documentation, mcp-server
├── packages/      telemetry-engine, trip-engine, scoring-engine,
│                  geospatial-engine, event-detection, shared-types
├── sdk/           ios, android, react-native, flutter, javascript, python
├── examples/      delivery-tracking, fleet-management, personal-driving, demo-app
├── infrastructure/ docker, aws, terraform
├── tests/
├── docs/
├── .github/
├── LICENSE (Apache 2.0)
├── README.md, SECURITY.md, CONTRIBUTING.md, CHANGELOG.md
└── docker-compose.yml
```

## 4. Core telematics engine

Configurable engine for collecting, processing, and analyzing smartphone-based telemetry.

### Location intelligence

GPS coordinates · accuracy · elevation · heading/bearing · speed · distance travelled · route history · trip duration · stop duration · idle time estimation · geofencing · location freshness indicators.

### Motion intelligence

Accelerometer · gyroscope · gravity · device orientation · motion activity · vehicle movement estimation · sensor availability and quality indicators.

### Transportation mode detection

Stationary · Walking · Running · Bicycle · Motor vehicle · Unknown — always return **classification confidence**.

- Never assume a phone belongs to the vehicle **driver** merely because the device is in a car.
- Allow users to correct transportation mode and driver/passenger classification.

### Automatic trip detection

Automatic start/end · manual recording · configurable thresholds · segmentation/merging · false-positive filtering · pause/resume · offline recording · recovery after interruption · user trip classification.

**State machine:**

```text
IDLE → MOVEMENT_DETECTED → TRIP_CANDIDATE → TRIP_ACTIVE
  → TRIP_PAUSED → TRIP_COMPLETED → PROCESSING → PROCESSED
```

## 5. Offline-first mobile SDK

Record without active internet when OS permissions and background execution permit.

- Encrypted local telemetry storage
- Persistent upload queues
- Auto sync after reconnection
- Airplane-mode-compatible collection where supported
- Batched + compressed uploads
- Retry with exponential backoff
- Duplicate prevention
- Interrupted upload recovery
- Timestamp validation
- Data quality monitoring
- Battery-aware sampling
- Adaptive GPS frequency
- Permission and tracking status reporting

**Do not** bypass OS privacy restrictions, disabled location services, revoked permissions, or force-stop restrictions.

Clearly communicate when tracking is active, interrupted, or unavailable.

**Explicit tracking modes:** Manual trip · Active delivery · Active work shift · Opt-in automatic trip detection.

Default to **purpose-limited** collection—not unrestricted continuous surveillance.

## 6. Driving behavior intelligence

Configurable detection of: harsh braking · rapid acceleration · harsh cornering · excessive speed (only where verified speed-limit data is available) · frequent stops · abnormal movement · device handling events (where supported) · possible collision events.

Require: sensor orientation normalization · noise filtering · GPS accuracy filtering · confidence scoring.

Thresholds configurable by application, fleet, and transportation mode.

- Do **not** auto-classify bumps, potholes, phone drops, or GPS errors as confirmed collisions.
- Collision detection is **experimental** until extensively validated.

## 7. Driver scoring engine

Transparent, configurable scoring. Initial example weights:

| Component | Weight |
|---|---|
| Braking | 25% |
| Acceleration | 20% |
| Cornering | 20% |
| Speed compliance | 20% |
| Distraction indicators | 15% |

Return: overall safety score · component scores · confidence · event frequency · distance-adjusted performance · historical comparisons · explainable methodology · algorithm version.

Account for trip distance, data quality, and sensor availability.

- Do **not** penalize drivers for unavailable sensor data.
- Do **not** present experimental scores as actuarially validated insurance risk assessments.

## 8. REST API

Complete **versioned** REST API with machine-readable **OpenAPI 3.1** (`GET /openapi.json`).

Cross-cutting requirements: **pagination**, **filtering**, **scoped authorization**, **idempotency**, **structured errors**, **API versioning**, **rate limits**.

### Authentication and projects

```text
POST /v1/auth/register
POST /v1/auth/login
POST /v1/auth/refresh
GET  /v1/projects
POST /v1/projects
POST /v1/projects/{id}/api-keys
GET  /v1/projects/{id}/usage
```

### Device management

```text
POST   /v1/devices
GET    /v1/devices
GET    /v1/devices/{id}
PATCH  /v1/devices/{id}
DELETE /v1/devices/{id}
GET    /v1/devices/{id}/status
GET    /v1/devices/{id}/location
```

### Telemetry ingestion

```text
POST /v1/telemetry/batch
POST /v1/telemetry/events
GET  /v1/telemetry/status
```

### Trip management

```text
POST   /v1/trips/start
POST   /v1/trips/{id}/end
GET    /v1/trips
GET    /v1/trips/{id}
GET    /v1/trips/{id}/route
GET    /v1/trips/{id}/events
GET    /v1/trips/{id}/score
PATCH  /v1/trips/{id}/classification
DELETE /v1/trips/{id}
```

### Driver intelligence

```text
GET /v1/drivers/{id}/score
GET /v1/drivers/{id}/analytics
GET /v1/drivers/{id}/events
GET /v1/drivers/{id}/trips
```

### Fleet management

```text
POST /v1/fleets
GET  /v1/fleets
GET  /v1/fleets/{id}/analytics
GET  /v1/fleets/{id}/devices
GET  /v1/fleets/{id}/events
```

### Geofences and webhooks

```text
POST   /v1/geofences
GET    /v1/geofences
DELETE /v1/geofences/{id}
POST   /v1/webhooks
GET    /v1/webhooks
DELETE /v1/webhooks/{id}
GET    /v1/webhooks/{id}/deliveries
```

### Platform

```text
GET /health
GET /ready
GET /v1/version
GET /v1/capabilities
GET /openapi.json
```

See also [`API.md`](API.md).

## 9. Real-time events and webhooks

Signed webhook notifications for:

- `trip.started` · `trip.completed` · `trip.processed`
- `device.location.updated` · `device.offline` · `device.online` · `tracking.interrupted`
- `geofence.entered` · `geofence.exited`
- `driving.harsh_braking` · `driving.harsh_acceleration` · `driving.harsh_cornering`
- `collision.suspected`
- `driver.score.updated`

Implement: HMAC signatures · timestamp verification · replay protection · delivery retries · dead-letter queues · webhook delivery logs · event deduplication · secret rotation · **SSRF protection**.

Provide **WebSocket or SSE** for authorized live dashboards.

## 10. MCP server

Independent Aimlake Telematics MCP server (`apps/mcp-server`), compatible with **AIVAS** and other standards-compatible MCP clients.

Must use the **same authenticated service layer** as the REST API.

- **Read-only by default**
- Explicit authorization for data exports, destructive actions, and sensitive location access
- Typed input schemas and structured responses

### Tools

```text
list_devices
get_device_status
get_current_location
list_trips
get_trip_details
get_trip_route
get_driver_score
get_driver_analytics
get_fleet_analytics
get_driving_events
list_geofences
create_geofence
delete_geofence
generate_trip_report
export_telemetry
```

See also [`MCP.md`](MCP.md).

## 11. Developer console

Premium, modern developer dashboard (`apps/dashboard`). Responsive, accessible, visually refined. Light and dark modes. Meaningful charts and maps—avoid metric overload.

### Navigation

Overview · Projects · API keys · Devices · Live map · Trips · Drivers · Fleets · Geofences · Events · Analytics · Webhooks · API playground · Usage · Team · Settings · Documentation

### Overview widgets

Active devices · Active trips · Completed trips · Distance travelled · Driving events · API request volume · Data ingestion volume · System health

See [`CONSOLE.md`](CONSOLE.md).

## 12. Interactive API playground

Postman-style tester inside the developer console.

- Endpoint selection · HTTP methods · headers · body editor · auth · JSON response · status · response time · code generation · request history
- Code examples: cURL · JavaScript · Python · Swift · Kotlin · PHP · Go
- Isolated **sandbox** with synthetic devices, trips, routes, events
- **Do not** expose real customer information in the public demo

## 13. Open-source self-hosting

Entire core under Apache 2.0. Capacity limited only by the operator’s infrastructure.

Self-hosted must include: unlimited software-level devices & API requests · configurable retention · all core telematics features · developer dashboard · REST API · MCP · SDKs · webhooks · fleets · geofences · driver analytics.

**Forbidden in self-hosted core:** mandatory Aimlake authentication · remote licensing verification · forced cloud connectivity · feature locks · installation analytics without explicit opt-in.

### Simple install (URL provisional)

```bash
git clone https://github.com/aimlake/telematics.git   # update when published
cd telematics
cp .env.example .env
docker compose up -d
```

Also provide: installation wizard · initial admin setup · migrations · secure secret generation · HTTPS guidance · backup/restore · upgrades · ARM64 + AMD64 images · env docs.

**Minimal dependencies.** Must remain usable **without** Amazon AWS, Google Maps, OpenAI, or any other commercial service. MapLibre + configurable self-hosted/public tile providers.

See [`SELF_HOSTING.md`](SELF_HOSTING.md).

## 16. Security and privacy

Security from the first development phase. See [`SECURITY.md`](SECURITY.md) (repo root) and details below.

### Requirements

- Strict tenant isolation · organization-based access control · scoped API keys · hashed API credentials
- Short-lived access tokens · refresh token rotation · device-specific credentials
- Encryption in transit · encryption at rest · input validation · SQL injection prevention · rate limiting
- Audit logging · webhook authentication
- Dependency scanning · container vulnerability scanning · secret scanning
- Secure password storage · account deletion · data export
- Consent records · consent revocation · configurable retention · automated data deletion

Support privacy needs for Canadian and international deployments **without claiming automatic legal compliance**.

Clear user-facing explanation of: collected data · purposes · retention periods.

- **Do not** allow covert tracking
- **Do not** log sensitive GPS coordinates in general application logs
- Implement **automated cross-tenant authorization tests**

## 19. Testing and benchmarking

Create a comprehensive **telemetry simulator** (scenarios under `tests/` / examples as implemented).

### Simulated scenarios

Walking · Cycling · Urban driving · Highway driving · Stop-and-go traffic · Hard braking · Rapid acceleration · GPS signal loss · Offline recording · Duplicate requests · Delayed synchronization · Invalid sensor values · Device permission changes

### Benchmarks

Trip detection accuracy · False-positive rates · Transportation classification accuracy · Battery consumption · API response latency · Ingestion throughput · Storage consumption · Webhook delivery reliability · Offline synchronization reliability

- Publish **reproducible** benchmark results
- **Do not** publish unverified performance or accuracy claims

See [`TESTING.md`](TESTING.md).

## 20. Open-source release quality

Professional OSS standards before/as public release.

Include: professional README · architecture diagrams · quick-start · API reference · SDK docs · CONTRIBUTING · security disclosure · Code of Conduct · CHANGELOG · issue templates · PR templates · semantic versioning · automated releases · SBOM · signed release artifacts where practical · automated tests · public roadmap

All core SDKs, dashboard, server components, and deployment scripts need clear **licensing and dependency documentation**.

See [`RELEASE_QUALITY.md`](RELEASE_QUALITY.md).
