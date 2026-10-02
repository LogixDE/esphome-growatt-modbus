# Contributing

Contributions are welcome. For bug reports, please include:

- Growatt inverter model
- inverter firmware version
- ESPHome version
- ESP32/RS485 hardware
- whether normal Modbus registers and FC32 are affected
- a focused DEBUG log excerpt

For register-map changes, include the source or a reproducible observation and clearly state whether the value is signed/unsigned, 16/32-bit and its scaling factor.

Please avoid committing personal Wi-Fi credentials, API keys, serial numbers or other secrets.

## Pull requests

1. Branch from `main`.
2. Keep changes focused.
3. Update the README when behaviour or configuration changes.
4. Update `CHANGELOG.md` for user-visible changes.
5. Ensure the ESPHome CI workflow passes.
