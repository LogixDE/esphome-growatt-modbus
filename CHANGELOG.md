# Changelog

All notable changes to this project will be documented in this file.

## [1.1.0] - 2026-11-23

### Added
- Tiered polling: separate periods for fast values, temperatures, energy counters, settings and the smart meter (`fast_poll_ms`, `temperature_poll_ms`, `energy_poll_ms`, `settings_poll_ms`, `meter_poll_ms`); missed ticks are coalesced, no polling during writes.
- Write queue (up to 10 jobs) with read-modify-write of time windows at execution time, so quick successive edits do not overwrite each other.
- `Inverter operating enable` switch (holding register 0) with readback and diagnostic `Inverter operating enable state`.
- Battery derating code 1 is resolved to "Standby", "System fault" or "Standby or fault (code 1)" from the other status values.
- `PV energy today` / `PV energy total`: PV production calculated as the sum of the PV1–PV3 tracker registers.
- `PV energy total (register)` (diagnostic): Growatt `Epv_total` register 91/92 for comparison; not populated on every model.

### Changed
- Writable numbers and switches are no longer optimistic; the state is published only after the write job is accepted.
- Documented that registers 53/55 (`Today/Total generate energy`) are the inverter's AC output and include battery discharge. Entity names are unchanged so existing Home Assistant entity IDs keep working.

## [1.0.1] - 2026-11-18

### Changed
- Set ESPHome `project` metadata so Home Assistant shows the device as "by Fabian Harder" (model "Growatt Modbus") instead of "by Espressif".

## [1.0.0] - 2026-10-02

- Document tested hardware: Waveshare ESP32-S3-Relay-6CH.

### Added
- Native Home Assistant integration through ESPHome API.
- Growatt inverter, PV and battery register mapping.
- Growatt smart-meter FC32 (`0x20`) support.
- Heartbeat-first polling and offline/unavailable handling.
- Verified Modbus writes with readback and retries.
- Stable optimistic UI behaviour while writes are being verified.
- Nine configurable priority/time windows.
- GitHub package example and CI compile check.

### Fixed
- FC32 byte-count offset handling.
- 32-bit smart-meter register offsets.
- ESPHome 2026.8 callback/API compatibility.
- Invalid template-select `unknown` state while the inverter is offline.
- Deprecated `register_count` configuration for DWORD sensors.
- Deprecated custom-command queue use for FC32.
