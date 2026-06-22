# Changelog

All notable changes to this project are documented here. This project aims to
follow [Semantic Versioning](https://semver.org/).

## [1.0.0] — 2026-06-21

First public release. Condition-aware autonomous mowing for Yarbo on Home
Assistant. Built against YarboHA v0.3.3 / `yarbo-data-sdk 0.2.1`.

### Added
- Single-source decision logic (`sensor.yarbocadenceeq_cut_blocker`) returning a
  plain-English reason list, with thin `ok_to_cut_now` / `status_reason` wrappers.
- Persistent **dock latch** (`input_boolean.yarbocadenceeq_docked`) set on charge and
  cleared only on GPS-confirmed departure — robust against the absence of any native
  "is docked" boolean and against battery-care clearing the charging flag.
- Configurable **moisture gate**: comma-separated sensor entity-ID list (UI field,
  255-char cap), on/off toggle, max-% limit, wettest-zone evaluation, and a
  block-and-notify path when the gate is on but all sensors are unavailable.
- **Onboard rain** detection as a standalone signal: factory rain bool OR a 30-second
  rolling-average threshold, persisted by a debounce to reject the head-startup spike.
- **Forecast-aware scheduling**: pre-rain pull-ahead only when a cut can finish before
  the rain; otherwise hold or schedule past rain-clear.
- Optional **dock-GPS gate** (within 0.5 m of saved coordinates).
- **Reset-to-recommended** defaults, applied on first install and on demand.
- Heat protection (pause/dock above threshold only within the direct-sun window;
  configurable resume mode).
- Four transient command scripts (start / stop / resume / go-home) plus refresh-all,
  HomeKit-friendly.
- Desktop (3-column) and phone Lovelace dashboards.

### Notes
- Charging is no longer a dispatch blocker; a cut may start at the battery minimum
  even mid-charge. Set the minimum to 100 to require a full battery.
- Forecast is used for **timing**; the onboard sensor is the authority for stopping a
  cut. Multi-source forecast cross-checking and a full forecast/onboard role split are
  planned for 2.0 (see ROADMAP.md).
