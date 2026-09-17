# Hotel Access Controller Development

Development Home Assistant add-on for running the Hotel Access Controller service on an `amd64` Home Assistant OS host.

The active development/staging SaaS origin is
`https://staging.autostay360.com`.

The administrator-only ingress UI provides secure factory Controller status and
provisioning-bundle import, Device Setup for Z-Wave locks and extenders, and
Offline Cache diagnostics. Existing manually configured installations remain
supported; factory installations can start with no Controller ID or token.

Release `0.1.0-dev.57` corrects the authenticated inventory-upload result path
and projects HAOS-confirmed security state into the SaaS
commissioning view while retaining the generic Controller Capability Manager and
adds guarded lock-code write recovery: an ambiguous transport failure or a
confirmed-empty read-back can replay only the same slot and PIN once. It uses a
packaged allowlist and `hassio_role: manager` without Docker access, full
access, an admin role, or unprotected mode. Existing Z-Wave JS is observed
only; no additional protocol runtime is bundled.

See `DOCS.md` for configuration and installation instructions.
