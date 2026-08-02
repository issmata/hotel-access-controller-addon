# Changelog

## 0.1.0-dev.3

- Enroll the Controller add-on in Home Assistant automatic updates during startup.
- Report installed, latest, available, and automatic-update state to SaaS without exposing Supervisor credentials.
- Use the add-on manifest as the authoritative runtime version.
- Publish catalog metadata only after the matching public GHCR image is verified.

## 0.1.0-dev.2

- Cache signed SaaS-authorized future access operations for local outage execution.
- Protect PIN-bearing manifests and execution journals under add-on `/data` with an installation-specific key.
- Reconcile durable offline results before ordinary command polling after reconnect.
- Report secret-free offline cache health and fail closed for stale data, corruption, or unsafe clock rollback.

## 0.1.0-dev.1

- Package the existing long-running controller worker as an `amd64` Home Assistant development add-on.
- Use the Supervisor-provided Home Assistant API proxy and token.
- Persist command results, pending acknowledgements, and diagnostics under `/data`.
- Add an internal health endpoint and native Docker health check.
