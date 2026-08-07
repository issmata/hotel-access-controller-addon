# Hotel Access Controller Development

This development add-on runs the existing Hotel Access Controller worker continuously. It polls Hotel Access SaaS for due commands and executes lock operations through Home Assistant and Z-Wave JS.

## Configuration

- `saas_base_url`: staging SaaS URL, without a trailing API path.
- `controller_id`: optional legacy Controller slug. Keep the existing value on
  an upgraded manually configured installation.
- `controller_token`: optional legacy agent bearer token. Keep the existing
  value paired with `controller_id`; never copy a real token into an image.
- `polling_interval_seconds`: queue polling cadence; defaults to 10 seconds.
- `heartbeat_interval_seconds`: controller check-in cadence; defaults to 60 seconds.
- `request_timeout_seconds`: outbound request timeout.
- `log_level`: `debug`, `info`, `warning`, or `error`.

The Home Assistant URL and token are supplied automatically through the Supervisor API proxy. Do not create a Home Assistant long-lived token for this add-on.

An add-on with empty Controller identity starts in `factory_unprovisioned`
standby. Open **Controller** in ingress to import a signed provisioning bundle.
It then reports `online_unclaimed` until SaaS adopts it and delivers permanent
credentials, after which it reports `claimed`. SaaS is the only component that
creates the permanent token. Bootstrap state and credentials are encrypted
under `/data/bootstrap` and survive restart. Identity reset does not reset the
Z-Wave network.

The official staging provisioning trust is packaged read-only at
`/app/config/trusted-provisioning-jwks.json`. Home Assistant has no editable
JWKS option. Ingress reports whether that artifact is valid and provides the
visual sequence from bundle selection through signature validation, appliance
identity generation, public-key registration, phone-home, and
`online_unclaimed`. Unknown keys, private material, wrong environments, and
unsupported cryptographic profiles are rejected without displaying secrets.

## Automatic updates

Starting with `0.1.0-dev.3`, the add-on uses the Supervisor's self-scoped API to enable automatic updates for itself at startup. It requests the default Supervisor role, cannot manage other add-ons, and does not receive host, Docker, or store-management access. Installed version, latest known version, update availability, and automatic-update state are sent to SaaS as secret-free controller check-in telemetry.

An installation older than `0.1.0-dev.3` cannot enable this behavior retroactively. Update it to `0.1.0-dev.3` once through the Home Assistant update action or an operator-assisted installation. Later catalog releases are installed by Supervisor automatically after Home Assistant discovers them. SaaS raises an alert if an update is pending or automatic updates become disabled.

## Operation

The add-on starts `php /app/bin/service` and the independent `php /app/bin/commissioning-worker`. Generated configuration, command result cache, pending acknowledgements, service lock, diagnostics state, redacted commissioning state, the encrypted offline manifest, and its encrypted execution journal are stored under `/data` and survive add-on and HAOS restarts.

The offline schedule follows the PMS future-booking window configured in SaaS. It is not a separate add-on option. While SaaS is unreachable, the Controller executes only signed operations already authorized in that manifest at their explicit UTC times. It does not store complete bookings or guest identity, contact, payment, or photo-ID data.

After reconnecting, unreported offline journal entries are uploaded before ordinary command polling. SaaS acknowledges reconciliation and explicitly supersedes stale local operations.

Select **Open Web UI** on the add-on to open **Device Setup**. The administrator-only wizard commissions Z-Wave locks and extenders with explicit security and history choices, live progress, safe security prompts, capability checks, and friendly naming. Previously used or uncertain devices must complete exclusion before inclusion. A lock is uploaded to discovered-lock mapping only after verification and temporary-PIN cleanup succeed.

Hotel-facing Device Setup is performed in SaaS. Release `0.1.0-dev.19` exposes
door lock, Z-Wave extender, and device exclusion flows through outbound-polled
commissioning commands while reusing this same commissioning service and
worker. A newly included lock receives a bounded Home Assistant registration
grace period, and a completed lock interview is not repeated solely because
its entity has not appeared yet. Extender progress reports plain-language routing verification and only
a bounded opaque completion identity. The local Device Setup page remains an
administrator-only factory and support tool; hotel users never need Home
Assistant, ingress, add-on settings, Controller credentials, or local network
access.

The **Offline Cache** tab shows cache health and redacted operation details including the lock, action, UTC execution time, state, slot, command ID, opaque booking/credential references, dependencies, and whether encrypted PIN material is present. Cached PIN values, DSK values, security keys, signatures, tokens, test PINs, and guest identity never appear.

The Controller, Device Setup, and Offline Cache tabs remain available from every
ingress page and are rooted beneath Home Assistant's active ingress URL.

The image's native Docker health check uses the minimal internal `/health` endpoint. Secret-free JSON diagnostics remain available separately at `/diagnostics` inside the container.

Home Assistant's default automatic boot policy is used. The obsolete manifest
`watchdog` setting is intentionally omitted because the image supplies its own
native Docker `HEALTHCHECK` against `/health`.

Scheduling remains authoritative in SaaS. Future operations are not returned by the due-command endpoint until their `available_at` time.
