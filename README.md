# ESPHome Growatt Modbus

[![ESPHome compile](https://github.com/LogixDE/esphome-growatt-modbus/actions/workflows/esphome.yml/badge.svg)](https://github.com/LogixDE/esphome-growatt-modbus/actions/workflows/esphome.yml)

[Deutsche README](README.de.md)

ESPHome integration for Growatt hybrid inverters via RS485/Modbus RTU, with native Home Assistant entities, Growatt smart-meter support through function code `0x20`, offline detection, and verified writes.

> Community project. Not affiliated with or endorsed by Growatt. Writing inverter registers can change charging and operating behaviour. Verify register addresses for your inverter/firmware before enabling write controls.

## Highlights

- Native ESPHome API / Home Assistant entities — no MQTT required
- Inverter, PV string, grid and battery measurements
- Battery charge/discharge settings
- AC charging control
- Nine Growatt priority/time windows
- Growatt smart meter via proprietary/custom Modbus function code `0x20`
- Heartbeat-first polling: only polls the full register set when the inverter responds
- Home Assistant values become unavailable when the inverter is offline
- Write → readback → verify with retries for settings
- Smart-meter FC32 byte-count handling

## Tested hardware

This project was developed and tested on a **Waveshare ESP32-S3-Relay-6CH**. The board is especially convenient for this application because it combines the ESP32-S3, an isolated onboard RS485 interface, wide-range DC power input and six relays in one DIN-rail-capable module.

- **Board:** Waveshare ESP32-S3-Relay-6CH
- **Product used:** [Amazon.de – B0FNVWFZ4Z](https://www.amazon.de/dp/B0FNVWFZ4Z)
- **Manufacturer documentation:** [Waveshare ESP32-S3-Relay-6CH documentation](https://docs.waveshare.com/ESP32-S3-Relay-6CH)
- **ESPHome:** tested with 2026.9.1
- **Growatt communication:** Modbus RTU, slave address `1`
- **UART:** 38400 baud, 8N1
- **RS485 pins on the tested Waveshare board:** GPIO17 TX / GPIO18 RX

> **Hardware photo placeholder**  
> A photo of the installed Waveshare ESP32-S3-Relay-6CH / Growatt controller will be added here.
> Suggested file: `docs/images/waveshare-growatt-controller.jpg`

The package can be adapted to other ESP32-S3/RS485 hardware through substitutions, but the Waveshare ESP32-S3-Relay-6CH is the hardware configuration used for development and testing. Other Growatt models and firmware versions may also use different registers. Please report confirmed working combinations in Issues.

## Wiring

Default package pins:

| ESP32-S3 | RS485 transceiver | Purpose |
|---|---|---|
| GPIO17 | DI / TX | UART transmit |
| GPIO18 | RO / RX | UART receive |
| GPIO21 | DE + /RE | Direction control |
| GND | GND | Common ground |

Connect the RS485 A/B pair to the inverter according to the inverter/transceiver documentation. Adapt the substitutions if your hardware uses different pins.

## Installation

1. Copy [`examples/growatt-example.yaml`](examples/growatt-example.yaml) to your ESPHome configuration directory.
2. Create `secrets.yaml` using [`examples/secrets.example.yaml`](examples/secrets.example.yaml) as a template.
3. Adjust UART pins, baud rate, Modbus address and device name.
4. Compile and flash with ESPHome.

The example loads the package directly from this repository and pins it to `v1.0.0`. Pinning a release is recommended so future changes on `main` do not unexpectedly change a working installation. ESPHome remote packages support Git repositories and per-file variables; remote packages intentionally contain no `!secret` lookups.

### Remote package structure

The example uses the repository form because the time-window template is instantiated nine times with different register addresses:

```yaml
packages:
  growatt:
    url: https://github.com/LogixDE/esphome-growatt-modbus
    ref: v1.0.0
    files:
      - path: packages/growatt.yaml
      - path: packages/growatt_zeitfenster.yaml
        vars: {period_num: "1", reg_addr: "3038", reg_addr_lo: "3039"}
      # ... periods 2 through 9; see examples/growatt-example.yaml
```

## Configuration substitutions

| Substitution | Default | Description |
|---|---:|---|
| `modbus_device_address` | `1` | Growatt Modbus slave address |
| `update_interval` | `5s` | Heartbeat/poll interval |
| `modbus_baud_rate` | `38400` | UART baud rate |
| `uart_tx_pin` | `GPIO17` | RS485 UART TX |
| `uart_rx_pin` | `GPIO18` | RS485 UART RX |
| `uart_flow_control_pin` | `GPIO21` | RS485 DE/RE direction control |

## Communication behaviour

A lightweight heartbeat reads inverter register `0` first. Only after a successful response does the package trigger the larger inverter/battery poll and the smart-meter FC32 request. If the inverter stops responding, `Growatt Modbus Online` turns off and dependent values are invalidated instead of leaving stale values in Home Assistant.

## Verified writes

Writable settings use a write/readback workflow. The requested value is shown immediately in Home Assistant, while the package verifies the actual register value in the background. On a final verification failure, the UI is restored to the value read from the inverter.

This is used for battery settings, AC charging and the time-window controls. The goal is to avoid silent lost commands on a busy or disturbed RS485 bus.

## Smart meter / FC32

Growatt smart-meter data is read with custom Modbus function code `0x20` (FC32). The response contains a byte-count field before the register payload. The package validates and skips this byte before decoding 32-bit values.

Mapped smart-meter values include phase voltage/current/power, total grid import/export power and total imported/exported energy.

## Time windows

Nine priority windows are supported. Each window stores a packed 32-bit value across two holding registers:

- bit 31: enabled
- bits 29–30: priority (`load`, `battery`, `grid`)
- bits 24–28: start hour
- bits 16–23: start minute
- bits 8–12: end hour
- bits 0–7: end minute

The register pairs used in this project are documented in [`examples/growatt-example.yaml`](examples/growatt-example.yaml).

## Troubleshooting

### `Growatt Modbus Online` is off

Check A/B polarity, common ground, slave address, baud rate, UART pins and the DE/RE direction pin.

### Smart-meter values are extremely large

Make sure you are using v1.0.0 or newer. Older development versions decoded the custom FC32 response one byte out of alignment.

### A setting appears to be ignored

Enable DEBUG logging and search for `growatt_write`. The package retries and verifies writes by reading the register back.

## Development

A GitHub Actions workflow compiles [`examples/growatt-ci.yaml`](examples/growatt-ci.yaml) against ESPHome 2026.9.1 on every push and pull request.

```bash
python -m pip install esphome==2026.9.1
esphome config examples/growatt-ci.yaml
esphome compile examples/growatt-ci.yaml
```

## Contributing

Bug reports and confirmed model/register information are welcome. See [`CONTRIBUTING.md`](CONTRIBUTING.md). Please include inverter model, firmware version, ESPHome version and relevant DEBUG log excerpts.

## License

MIT — see [`LICENSE`](LICENSE).

## Disclaimer

Use at your own risk. This project can write inverter configuration registers. Incorrect settings can affect charging, battery behaviour or inverter operation. Always verify the register map for your exact device and firmware.
