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

## Operation

The add-on starts `php /app/bin/service`. Generated configuration, command result cache, pending acknowledgements, service lock, and diagnostics state are stored under `/data` and survive add-on restarts and upgrades.

The Supervisor watchdog checks the internal `/health` endpoint. Scheduling remains authoritative in SaaS. Future operations are not returned by the due-command endpoint until their `available_at` time.
