# Troubleshooting

## `Growatt Modbus Online` stays off

Check, in this order:

1. inverter power and RS485 wiring;
2. A/B polarity;
3. common GND where required by the transceiver;
4. slave address (default `1`);
5. baud rate (default `38400`, 8N1);
6. DE/RE direction control;
7. UART TX/RX pin assignment.

With no inverter response, heartbeat timeouts are expected. Full polling is gated behind the heartbeat.

## Standard inverter values work, Smart Meter values do not

The Smart Meter does not use normal FC3/FC4 reads. This project uses custom function code `0x20` and expects a response with a byte-count field followed by big-endian register data. Enable DEBUG logging and look for `growatt_fc32` messages.

If values are wildly unrealistic while normal inverter values look correct, include the FC32 log lines and raw response size/byte-count in an issue.

## A write appears to be ignored

Writes are verified by reading the register back. Enable DEBUG logging and look for `growatt_write` messages. The package retries mismatches/timeouts and performs a final read-back.

If the final value still differs, collect the log around the operation and note the requested and actual value.

## Home Assistant briefly shows unavailable values

When the heartbeat declares the inverter offline, the package intentionally publishes invalid sensor states rather than leaving stale measurements visible. Values return after communication is restored and a new poll succeeds.

## Time window does not update

Each time window consists of two 16-bit words. Both must be readable before edits are accepted, and both words are verified after writing. Ensure the inverter is online and wait for the first successful data poll after startup.

## Recommended issue data

Please include:

- ESPHome version;
- inverter model and firmware if known;
- RS485 transceiver model/type;
- UART pins and baud rate;
- whether FC3/FC4 reads work;
- whether FC32 works;
- a DEBUG log excerpt covering the failure.
