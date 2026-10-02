# Changelog

All notable changes to this project will be documented in this file.

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
