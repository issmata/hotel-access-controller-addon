# Hotel Access Controller Development

Version `0.1.0-dev.56` is the Home Assistant OS add-on that connects a hotel
Controller to Hotel Access SaaS. It runs continuously, checks in with SaaS,
polls for due work, and uses Home Assistant and Z-Wave JS locally to carry out
approved Controller operations.

The Controller initiates all SaaS communication. SaaS does not open a network
connection into the hotel.

## What this add-on includes

- **Controller identity and provisioning.** Existing manually configured
  Controller IDs and tokens are retained on update. A new factory Controller
  can be provisioned from a signed bundle in the ingress UI; permanent identity
  is then delivered by SaaS.
- **Hotel access execution.** The worker receives due SaaS commands, writes and
  removes lock user codes through Home Assistant/Z-Wave JS, verifies the
  affected lock slot, and reports the result to SaaS.
- **Device Setup.** Home Assistant administrators can commission Z-Wave locks
  and extenders, run exclusion where required, follow secure inclusion prompts,
  name devices, and validate a newly included lock before it is sent to SaaS.
- **Offline Access.** The Controller stores a signed, encrypted schedule of
  already-authorized future lock operations. During a SaaS outage it executes
  only those due operations, then reconciles its encrypted journal when SaaS is
  reachable again.
- **Controller recovery.** When enabled and initiated by SaaS, the existing
  recovery workflow can create and restore supported Controller/Z-Wave recovery
  points without replacing Controller identity.
- **Controller Capability Manager.** The generic framework accepts only
  revisioned, allowlisted desired state from SaaS and records secret-free local
  reconciliation state. It currently observes the existing Z-Wave JS runtime;
  see [Capability Manager](#capability-manager) below.
- **Automatic self-updates.** The add-on keeps its own Supervisor automatic
  updates enabled and reports its version/update state to SaaS.

## What this add-on does not include

- No LoRaWAN, BLE, Zigbee, HKT, Wi-Fi vendor, or other protocol driver.
- No arbitrary add-on installer, repository URL, Supervisor path, shell
  command, environment-variable pass-through, or secret-delivery mechanism.
- No Docker socket, SSH, host PID access, host networking, full access, admin
  Supervisor role, or unprotected mode.
- No customer-facing Home Assistant workflow. Hotel staff use Hotel Access
  SaaS; local ingress is for Home Assistant administrators and support.

## Install or update

1. In Home Assistant, open **Settings > Apps > App store**.
2. Add the repository `https://github.com/issmata/hotel-access-controller-addon`
   if it is not already present, then refresh the store.
3. Install or update **Hotel Access Controller Development**.
4. Keep the existing `controller_id` and `controller_token` on an upgraded,
   already-claimed Controller. Do not reset the Controller or its Z-Wave
   network during a routine upgrade.
5. For a new factory Controller, leave both identity fields empty. Start the
   add-on, open **Open Web UI > Controller**, and import the signed provisioning
   bundle prepared in SaaS.
6. Start the add-on and enable Home Assistant's **Start on boot** and
   **Watchdog** controls for it.

The add-on image is `amd64` only. Its current development image is
`ghcr.io/issmata/hotel-access-controller-addon:0.1.0-dev.56`.

## Configuration

| Option | Use |
| --- | --- |
| `saas_base_url` | SaaS HTTPS base URL, with no trailing API path. The current development default is `https://staging.autostay360.com`. |
| `controller_id` | Existing legacy Controller slug. Leave blank for factory provisioning. |
| `controller_token` | Existing legacy Controller bearer token. Keep it paired with its Controller ID and never place it in an image or support message. |
| `polling_interval_seconds` | Due-command polling cadence; default `10`. |
| `heartbeat_interval_seconds` | SaaS check-in cadence; default `60`. |
| `request_timeout_seconds` | Outbound request timeout. |
| `log_level` | `debug`, `info`, `warning`, or `error`; start with `info`. |

Home Assistant supplies the Home Assistant and Supervisor credentials through
its protected Supervisor proxy. Do not create a Home Assistant long-lived token
for this add-on.

## Ingress: Open Web UI

The administrator-only ingress UI contains three practical areas:

- **Controller** shows provisioning and claim status. Use it on a factory unit
  to import a signed provisioning bundle and confirm its trust state.
- **Device Setup** is the local Z-Wave commissioning tool for locks and
  extenders. It uses explicit security/history choices, requires exclusion for
  previously used or uncertain hardware, and never overwrites occupied lock
  user-code slots during validation.
- **Offline Cache** shows secret-free cache health and operation summaries:
  lock, action, UTC time, state, slot, command ID, opaque references, and
  dependencies. PINs, DSKs, tokens, signatures, guest identity, and other
  secrets are never displayed.

Hotel-facing commissioning, normal lock access, scheduling, and access-policy
decisions remain in SaaS. The Controller does not decide whether a future
booking operation is allowed or due.

## Capability Manager

`0.1.0-dev.56` retains `controller.capability_manager.v1` in normal Controller
check-in metadata. SaaS can then send the existing command transport a bounded
`reconcile_controller_capabilities` desired-state manifest. The Controller
validates its revision and logical capability IDs, stores only secret-free
state under `/data/capabilities`, re-observes Supervisor state after restart,
and returns a bounded operational result.

Today, the only production capability is **Z-Wave**. The add-on treats the
existing `core_zwave_js` runtime as external: it may observe its operational
state, but it does not install, configure, update, restart, stop, or remove it.
This preserves the existing Controller identity, Z-Wave network, lock access,
Device Setup, and Cameo behavior.

The generic framework has no approved production runtime to install yet. Its
Supervisor-managed fixture is test-only. A future protocol runtime requires a
separate approved capability package, hardware integration, and rollout; it is
not enabled by adding an add-on option.

The add-on uses `hassio_role: manager` solely for the framework's fixed,
catalog-allowlisted Supervisor operations. It does not request higher
privileges. The exact cross-add-on operations remain subject to validation on a
supported HAOS Controller; a `403` must be recorded and investigated rather
than solved by elevating to `admin`.

For lock-code provisioning, the add-on verifies every write with an exact-slot
read. If SaaS requests a second write attempt, the add-on replays the same slot
and PIN only after that read still reports the slot empty. An ambiguous
Supervisor/Core request is also checked before replaying. It never overwrites
an occupied or unreadable slot, and an HTTP success alone never marks a PIN
active.

## Updates, persistence, and security

Home Assistant retains add-on options on update. The Controller keeps its
generated configuration, encrypted bootstrap identity, command/idempotency
cache, pending acknowledgements, offline schedule and journal, commissioning
state, recovery state, Capability Manager state, and diagnostics under `/data`.
It does not store its state in a local database.

At startup, the add-on enables Supervisor automatic updates for itself and
reports installed/latest versions, update availability, and automatic-update
state to SaaS. The Capability Manager does not take over Z-Wave JS updates.

The add-on remains in protected mode. Its native health check uses `/health`;
secret-free runtime diagnostics are available at `/diagnostics` inside the
container. Supervisor tokens, Controller tokens, lock PINs, guest data,
protocol secrets, raw Supervisor responses, and raw hardware inventory are not
included in SaaS telemetry or Capability Manager state.

## Verify a healthy installation

After installation or update, confirm:

1. The add-on log shows successful SaaS check-ins, Home Assistant reachability,
   and command polling.
2. The Controller remains claimed and online in SaaS.
3. Automatic updates remain enabled for the Controller add-on.
4. Existing Z-Wave control, lock discovery, and PIN create/read/remove work
   normally.
5. Offline Cache remains healthy and the Z-Wave network is unchanged.

For a `0.1.0-dev.56` canary, also verify that unknown capability IDs are
rejected, a no-op desired state is idempotent, a restart during reconciliation
does not duplicate a mutation, and a set PIN is reported active only after an
exact-slot read-back. Do not proceed to a fleet rollout until the supported-HAOS
`manager`-role checks and Cameo regression are complete.

## Support information

Use **Open Web UI** and the add-on log first. The `/diagnostics` response
contains secret-free connectivity, polling, check-in, pending-result, last
command, offline-cache, bootstrap, and application-version information.
Never attach Controller tokens, Supervisor tokens, PINs, DSKs, signed bundles,
or unredacted logs to a support request.
