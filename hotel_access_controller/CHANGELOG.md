# Changelog

## 0.1.0-dev.17

- Route SaaS-issued Z-Wave extender inclusion and exclusion through the existing durable Device Setup commissioning engine.
- Report sanitized routing verification, opaque extender identity, readiness, health, and completion summaries without exposing local platform or node identifiers.
- Advertise `commissioning.extender.v1` alongside `commissioning.saas_native.v1` only in the package that supports the complete retry, restart, routing, and exclusion flow.

## 0.1.0-dev.16

- Execute SaaS-native lock inclusion and exclusion through the existing durable commissioning engine.
- Relay restart-safe progress, cancellation, timeout, DSK confirmation, and explicit S2 grant prompts over the outbound Controller API.
- Advertise `commissioning.saas_native.v1` only when the complete command, persistence, prompt, event, and result bridge is available.
- Continue draining prioritized SaaS commands immediately while a bounded batch remains full.

## 0.1.0-dev.15

- Keep Controller, Device Setup, and Offline Cache navigation visible on the Offline Cache page.
- Root all Offline Cache links safely beneath the active Home Assistant ingress path.

## 0.1.0-dev.14

- Package the reviewed staging provisioning trust artifact at `/app/config/trusted-provisioning-jwks.json` and remove editable Home Assistant JWKS configuration.
- Pin environment, issuer, active kid, ES256/P-256 profile, validity windows, and canonical artifact hash; reject private, duplicate, unknown, malformed, or out-of-window keys.
- Add visual provisioning-bundle selection, safe metadata review, durable preparation progress, trusted-key status, and sanitized failures in administrator ingress.
- Verify the exact trust bytes inside the built image before GHCR publication while preserving existing Cameo credentials unchanged.

## 0.1.0-dev.13

- Allow a valid legacy Controller ID/token pair to take over from empty factory standby state.
- Retain explicit reset protection and active provisioning-bundle precedence.

## 0.1.0-dev.12

- Add secure ES256 factory provisioning and local P-256 recipient keys.
- Add authenticated phone-home, adoption polling, compact JWE credential delivery, persistence, verification, and acknowledgement.
- Start healthily with empty Controller identity while preserving legacy Cameo configuration unchanged.
- Add redacted bootstrap status and provisioning-bundle import to administrator-only ingress.

## 0.1.0-dev.11

- Open Device Setup at the welcome step after a completed cancellation.
- Keep the cancelled screen only while physical Home Assistant cleanup is still pending.

## 0.1.0-dev.10

- Use the configured add-on version for ingress asset cache busting.
- Remove source-tree asset path assumptions that caused `filemtime` warnings in the packaged container.

## 0.1.0-dev.9

- Reset terminal commissioning state on the Controller before opening a new Device Setup wizard.
- Prevent the status poll from restoring a cancelled session after selecting another device.

## 0.1.0-dev.8

- Present device type, security mode, and device history as sequential setup screens.
- Ensure inactive wizard controls remain hidden throughout commissioning.
- Make cancellation durable and prevent queued work from restarting a cancelled session.
- Use the supplied Locstar Z-Wave lock product photo in Device Setup.

## 0.1.0-dev.7

- Remove the unsupported `exclusion_strategy` field from `zwave_js/remove_node`.
- Match the exclusion command schema used by the installed Home Assistant Z-Wave JS integration.

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
