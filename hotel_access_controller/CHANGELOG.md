# Changelog

## 0.1.0-dev.1

- Package the existing long-running controller worker as an `amd64` Home Assistant development add-on.
- Use the Supervisor-provided Home Assistant API proxy and token.
- Persist command results, pending acknowledgements, and diagnostics under `/data`.
- Add an internal health endpoint for Home Assistant Supervisor watchdog checks.
