# Changelog

## 0.1.0-dev.6

- Add an administrator-only Device Setup ingress wizard for Z-Wave locks and extenders.
- Require explicit security and device-history choices, including mandatory exclusion for used or uncertain devices.
- Monitor inclusion, security prompts, interview and bounded re-interview through Home Assistant Z-Wave JS WebSocket APIs.
- Verify Hotel Access lock capabilities with a temporary in-memory PIN and verified cleanup before commissioning.
- Persist only redacted, non-secret commissioning state and upload successfully commissioned locks to discovered-lock mapping.
- Preserve Offline Cache diagnostics as a separate ingress view.

## 0.1.0-dev.5

- Add an administrator-only Home Assistant ingress page for inspecting the offline cache.
- Display redacted operation, lock, schedule, state, and opaque reference details without exposing PINs or secrets.
- Keep the health endpoint minimal while retaining full secret-free JSON diagnostics separately.

## 0.1.0-dev.4

- Bundle version-matched public catalog metadata inside the verified container image.
- Support credential-free pull-based catalog publication from the public add-on repository.

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
