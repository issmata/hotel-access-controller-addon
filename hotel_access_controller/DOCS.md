# Hotel Access Controller Development

This development add-on runs the existing Hotel Access Controller worker continuously. It polls Hotel Access SaaS for due commands and executes lock operations through Home Assistant and Z-Wave JS.

## Configuration

- `saas_base_url`: staging SaaS URL, without a trailing API path.
- `controller_id`: controller slug configured in SaaS.
- `controller_token`: controller agent bearer token from SaaS.
- `polling_interval_seconds`: queue polling cadence; defaults to 10 seconds.
- `heartbeat_interval_seconds`: controller check-in cadence; defaults to 60 seconds.
- `request_timeout_seconds`: outbound request timeout.
- `log_level`: `debug`, `info`, `warning`, or `error`.

The Home Assistant URL and token are supplied automatically through the Supervisor API proxy. Do not create a Home Assistant long-lived token for this add-on.

## Automatic updates

Starting with `0.1.0-dev.3`, the add-on uses the Supervisor's self-scoped API to enable automatic updates for itself at startup. It requests the default Supervisor role, cannot manage other add-ons, and does not receive host, Docker, or store-management access. Installed version, latest known version, update availability, and automatic-update state are sent to SaaS as secret-free controller check-in telemetry.

An installation older than `0.1.0-dev.3` cannot enable this behavior retroactively. Update it to `0.1.0-dev.3` once through the Home Assistant update action or an operator-assisted installation. Later catalog releases are installed by Supervisor automatically after Home Assistant discovers them. SaaS raises an alert if an update is pending or automatic updates become disabled.

## Operation

The add-on starts `php /app/bin/service`. Generated configuration, command result cache, pending acknowledgements, service lock, diagnostics state, the encrypted offline manifest, and its encrypted execution journal are stored under `/data` and survive add-on and HAOS restarts.

The offline schedule follows the PMS future-booking window configured in SaaS. It is not a separate add-on option. While SaaS is unreachable, the Controller executes only signed operations already authorized in that manifest at their explicit UTC times. It does not store complete bookings or guest identity, contact, payment, or photo-ID data.

After reconnecting, unreported offline journal entries are uploaded before ordinary command polling. SaaS acknowledges reconciliation and explicitly supersedes stale local operations. Diagnostics expose counts, revision, validity, and cache health only; cached PINs never appear there.

The image's native Docker health check checks the internal `/health` endpoint. The same endpoint exposes secret-free runtime diagnostics inside the container, including SaaS reachability, authentication, Home Assistant reachability, polling and heartbeat state, pending acknowledgements, the last command, and the last successful PIN verification.

Scheduling remains authoritative in SaaS. Future operations are not returned by the due-command endpoint until their `available_at` time.
