# Aimlake Telematics MCP server

**App:** `apps/mcp-server`  
**Status:** Planned — implement as needed.

## Rules

- Independent MCP server; same **authenticated service layer** as the REST API
- Compatible with **AIVAS** and standards-compatible MCP clients
- Typed input schemas and structured responses
- **Read-only by default**
- Explicit authorization required for: data exports, destructive actions, sensitive location access

## Tools

| Tool | Notes |
|---|---|
| `list_devices` | Read |
| `get_device_status` | Read |
| `get_current_location` | Sensitive location — explicit auth |
| `list_trips` | Read |
| `get_trip_details` | Read |
| `get_trip_route` | Sensitive location — explicit auth |
| `get_driver_score` | Read |
| `get_driver_analytics` | Read |
| `get_fleet_analytics` | Read |
| `get_driving_events` | Read |
| `list_geofences` | Read |
| `create_geofence` | Write — explicit auth |
| `delete_geofence` | Destructive — explicit auth |
| `generate_trip_report` | Export-like — explicit auth |
| `export_telemetry` | Export — explicit auth |
