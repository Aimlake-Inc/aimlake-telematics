# REST API surface (v1)

**Status:** Planned — implement as needed.  
**Spec:** OpenAPI 3.1 · served at `GET /openapi.json`  
**Cross-cutting:** pagination · filtering · scoped authorization · idempotency · structured errors · versioning · rate limits

## Auth & projects

| Method | Path |
|---|---|
| POST | `/v1/auth/register` |
| POST | `/v1/auth/login` |
| POST | `/v1/auth/refresh` |
| GET | `/v1/projects` |
| POST | `/v1/projects` |
| POST | `/v1/projects/{id}/api-keys` |
| GET | `/v1/projects/{id}/usage` |

## Devices

| Method | Path |
|---|---|
| POST | `/v1/devices` |
| GET | `/v1/devices` |
| GET | `/v1/devices/{id}` |
| PATCH | `/v1/devices/{id}` |
| DELETE | `/v1/devices/{id}` |
| GET | `/v1/devices/{id}/status` |
| GET | `/v1/devices/{id}/location` |

## Telemetry

| Method | Path |
|---|---|
| POST | `/v1/telemetry/batch` |
| POST | `/v1/telemetry/events` |
| GET | `/v1/telemetry/status` |

## Trips

| Method | Path |
|---|---|
| POST | `/v1/trips/start` |
| POST | `/v1/trips/{id}/end` |
| GET | `/v1/trips` |
| GET | `/v1/trips/{id}` |
| GET | `/v1/trips/{id}/route` |
| GET | `/v1/trips/{id}/events` |
| GET | `/v1/trips/{id}/score` |
| PATCH | `/v1/trips/{id}/classification` |
| DELETE | `/v1/trips/{id}` |

## Drivers

| Method | Path |
|---|---|
| GET | `/v1/drivers/{id}/score` |
| GET | `/v1/drivers/{id}/analytics` |
| GET | `/v1/drivers/{id}/events` |
| GET | `/v1/drivers/{id}/trips` |

## Fleets

| Method | Path |
|---|---|
| POST | `/v1/fleets` |
| GET | `/v1/fleets` |
| GET | `/v1/fleets/{id}/analytics` |
| GET | `/v1/fleets/{id}/devices` |
| GET | `/v1/fleets/{id}/events` |

## Geofences & webhooks

| Method | Path |
|---|---|
| POST | `/v1/geofences` |
| GET | `/v1/geofences` |
| DELETE | `/v1/geofences/{id}` |
| POST | `/v1/webhooks` |
| GET | `/v1/webhooks` |
| DELETE | `/v1/webhooks/{id}` |
| GET | `/v1/webhooks/{id}/deliveries` |

## Platform

| Method | Path |
|---|---|
| GET | `/health` |
| GET | `/ready` |
| GET | `/v1/version` |
| GET | `/v1/capabilities` |
| GET | `/openapi.json` |

## Webhook event types

`trip.started` · `trip.completed` · `trip.processed` · `device.location.updated` · `device.offline` · `device.online` · `tracking.interrupted` · `geofence.entered` · `geofence.exited` · `driving.harsh_braking` · `driving.harsh_acceleration` · `driving.harsh_cornering` · `collision.suspected` · `driver.score.updated`

Delivery: HMAC · timestamp · replay protection · retries · DLQ · logs · dedupe · secret rotation · SSRF protection. Live dashboards: WebSocket or SSE.
