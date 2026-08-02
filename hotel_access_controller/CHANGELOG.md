# Changelog

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
