# Changelog

## 0.1.0-dev.52

- Extend the default lock-code removal verification window to 12 attempts with a two-second settling interval, while retaining expected-PIN checks on every retry.

## 0.1.0-dev.51

- Verify a lock slot even when Home Assistant reports an HTTP error from the clear action, because Z-Wave may already have accepted the removal.
- Retry the removal only when a fresh read confirms the same expected PIN is still present; continue to fail closed for changed or unreadable slots.

## 0.1.0-dev.50

- Read and compare the current slot PIN before every removal attempt; refuse to clear a slot if its code changed or cannot be confirmed.
- Treat an already empty slot as an idempotent successful removal without issuing a clear command.

## 0.1.0-dev.49

- Refuse to program a lock user-code slot unless an immediate preflight read confirms that the slot is vacant.

## 0.1.0-dev.48

- Replace legacy and delta-derived offline cache projections with an authoritative signed full manifest before acknowledgement.
- Remove locally retained cancelled operations without changing Controller identity, Z-Wave state, commands, or execution journals.

## 0.1.0-dev.47

- Include the latest persisted offline manifest revision and cache summary in the next SaaS check-in after synchronization.

## 0.1.0-dev.46

- Migrate the exact retired staging controller-agent endpoint in encrypted adopted runtime credentials.
- Preserve Controller identity, token, adoption, inventory, polling, offline, command, and Z-Wave state during migration.
- Keep migrated-origin authentication failures visible without logging SaaS response bodies.

## 0.1.0-dev.45

- Retry acknowledgement of the current persisted offline manifest when SaaS reports no newer manifest.
- Fail visibly when SaaS reports no manifest and the Controller has no persisted cache.
- Keep acknowledgement failure diagnostics bounded to HTTP status and manifest revision.

## 0.1.0-dev.44

- Move the active staging SaaS and bootstrap issuer to `https://staging.autostay360.com`.
- Migrate only the retired staging default in existing Home Assistant options while preserving every customized option and legacy Controller credential.
- Package and pin the reviewed new-origin staging provisioning trust artifact.

## 0.1.0-dev.43

- Preserve bounded, redacted Home Assistant Supervisor backup failure details so SaaS recovery diagnostics identify actionable HAOS conditions.

## 0.1.0-dev.42

- Include the required UTC `reported_at` timestamp in every recovery progress report.
- Align recovery-point result metadata with the finalized Shared schema.

## 0.1.0-dev.41

- Align adapter discovery and NVM backup/restore payloads with the current Z-Wave JS Server protocol.
- Normalize Zooz manufacturer/product identifiers, region, network identity, and node inventory from a bounded state snapshot.
- Exclude repository-prefixed AutoStay add-on slugs from selective Home Assistant restore.

## 0.1.0-dev.40

- Capture a complete Home Assistant recovery artifact and restore it selectively without replacing the prepared AutoStay controller identity.
- Verify the post-restore Z-Wave network fingerprint and report unresolved node inventory separately.
- Fail closed when physical adapter identity metadata is unavailable instead of assuming a Zooz adapter.

## 0.1.0-dev.39

- Add SaaS-managed controller and Zooz Z-Wave recovery points, compatibility preflight, restore progress, and idempotent recovery execution.
- Grant the bounded Home Assistant Supervisor backup role required for recovery artifacts.

## 0.1.0-dev.38

- Import manufacturer, model, battery percentage, and Z-Wave device metadata
  from Home Assistant during discovered-lock commissioning uploads.
- Prefer hardware-signature Z-Wave identifiers while preserving node IDs and
  Home Assistant device IDs as separate fields.
- Keep metadata enrichment optional so missing registry data never blocks
  successful lock commissioning.

## 0.1.0-dev.37

- Use the provisioned runtime controller identity for discovered-lock uploads.
- Fail locally when no authoritative controller identity is available instead
  of submitting an invalid empty controller slug.

## 0.1.0-dev.36

- Add application-level diagnostics for discovered-lock upload failures.
- Retain sanitized request payloads, correlation IDs, response status/content
  type/body, decoded validation errors, and persisted commissioning failure
  state without changing commissioning behavior.

## 0.1.0-dev.35

- Complete lock commissioning after registration, naming, interview readiness,
  and lock-entity verification without performing User Code operations.
- Preserve discovered-lock capability metadata while keeping PIN testing out of
  the commissioning critical path.

## 0.1.0-dev.33

- Release the Hotel Access add-node subscription immediately after Home
  Assistant registers the included device, without stopping physical inclusion.
- Let the durable commissioning worker reconcile interview readiness after
  naming and device registration in either order.
- Cover name-first, readiness-first, restart, duplicate, and exactly-once
  finalization behavior without repeating Z-Wave or temporary-PIN work.

## 0.1.0-dev.32

- Restore the Home Assistant device-registration boundary as the durable SaaS
  naming step while the physical Z-Wave interview continues independently.
- Persist registration, naming, interview, and finalization facts so either
  naming/interview order and supported restart states converge safely.
- Begin capability and temporary-PIN verification only after the device is
  registered, its friendly name is applied, and its interview is complete.
- Reconcile duplicate naming, repeated completion, cancellation, timeout, and
  interrupted finalization without repeating inclusion, interview, PIN tests,
  or discovered-lock execution.

## 0.1.0-dev.31

- Keep Home Assistant's explicit `interview completed` event as the primary
  transition to naming.
- Recover when Home Assistant omits that event only after the included node has
  a matching registered device and reports ready status three times in a row.
- Record whether interview completion came from the event stream or the bounded
  stable-readiness fallback for commissioning diagnostics.

## 0.1.0-dev.30

- Keep the Home Assistant `zwave_js/add_node` event subscription open through
  the explicit `interview completed` event before asking SaaS for a name.
- Preserve the working SaaS-to-Home Assistant friendly-name update while
  preventing premature lock-entity and temporary-PIN verification.
- Acknowledge operator cancellation immediately and let event-stream cleanup
  finish independently of the SaaS UI.

## 0.1.0-dev.29

- End the blocking Home Assistant inclusion subscription as soon as the
  physical device is registered, matching Home Assistant's naming boundary.
- Publish `naming_required`, accept `commissioning_submit_name`, and apply the
  hotel-facing name through the existing naming and verification engine.
- Keep interactive command polling active when an older result acknowledgement
  is unavailable, and acknowledge late cancellation/timeout commands cleanly.

## 0.1.0-dev.28

- Require a newly included lock to remain Home Assistant-ready through a
  20-second stabilization window before any temporary PIN is written.
- Pace temporary PIN set verification, cleanup, and cleanup verification so a
  battery lock can finish each mutation before receiving the next operation.
- Retry a late user-code service through one additional stabilization window
  without resetting the Z-Wave Serial API.

## 0.1.0-dev.27

- Mirror Home Assistant's supported classic-inclusion finish sequence by
  stopping inclusion after interview readiness and before device naming.
- Allow a bounded 30-second Home Assistant device-registry convergence window.

## 0.1.0-dev.26

- Continue polling Controller commands when commissioning progress telemetry is temporarily unavailable.
- Ensure interactive cancellation can be fetched and executed even while an earlier progress event is awaiting acknowledgement.
- Include the ready-node-to-naming correction from dev.25.

## 0.1.0-dev.25

- Use Home Assistant's controller-node readiness as the bounded fallback when registration and interview-completed events are delayed.
- End the inclusion subscription without resetting the Serial API, resolve the registered device, and proceed to naming before lock-entity verification.
- Preserve the direct cancellation and documented `device.id` handling introduced in dev.24.

## 0.1.0-dev.24

- Read the Home Assistant device registry identifier from the documented `device registered` event field and advance a ready device to the Hotel Access naming step.
- Apply the friendly device name before waiting for Home Assistant's lock entity and running user-code capability verification.
- Use the registered device's in-memory readiness status, with a bounded device-registry fallback when the completion event is absent.
- Stop treating the final `Retrieving priority route` log line as evidence that the Z-Wave Serial API is blocked; Z-Wave JS does not emit a matching success log line.
- Stop inclusion or exclusion directly from the command process so cancellation completes without waiting for the commissioning worker event loop.

## 0.1.0-dev.23

- Detect newly included node readiness directly from Home Assistant controller state without waiting for device-registry registration.
- Recover once after 60 seconds without interview progress when an optional Z-Wave JS UI priority-route request blocks the Serial API.
- Require a fresh completed interview after silent-stall recovery before user-code slot verification.
- Document that Z-Wave JS UI automatically requests a priority route when a node becomes ready; Hotel Access does not request one.

## 0.1.0-dev.22

- Treat priority-route retrieval as optional and unrelated to lock inclusion, interview completion, and user-code verification.
- Detect an interview-ready lock when the Home Assistant inclusion event stream is blocked after node readiness.
- Recover the Z-Wave Serial API before user-code slot discovery without repeating the completed interview.
- Retry user-code verification once after recovery when the completed interview event arrived before the route request blocked the queue.
- Preserve occupied user-code slots throughout recovery and temporary-PIN verification.

## 0.1.0-dev.21

- Configure the Z-Wave JS driver to query existing user codes instead of clearing them during the initial interview.
- Require an interview-ready node and usable Home Assistant lock entity before testing and cleaning the temporary PIN.
- Scan occupied slots without modifying them and use only the first verified empty slot for the temporary PIN test.
- Keep routing-readiness verification for Z-Wave extenders.

## 0.1.0-dev.20

- Allow Home Assistant up to one minute to publish a newly interviewed lock entity before capability verification fails.
- Keep the successfully included node in the same commissioning run while Home Assistant completes asynchronous registration.

## 0.1.0-dev.19

- Wait for Home Assistant to expose a newly included lock entity before capability verification.
- Do not trigger a second full lock interview after Home Assistant already confirmed the first interview complete.
- Prevent slow battery-lock user-code scans from turning successful inclusion into a false timeout.

## 0.1.0-dev.18

- Fix Home Assistant Z-Wave inclusion by omitting `force_security` when an explicit inclusion strategy is selected.
- Send S2 security approval fields using Home Assistant's required `securityClasses` and `clientSideAuth` names.

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
