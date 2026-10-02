# Register map used by this project

This document describes the addresses implemented by the ESPHome package. It is a project map, **not** a claim that every Growatt inverter uses the same addresses.

## Standard input registers (FC4)

| Entity | Address | Words | Scale |
|---|---:|---:|---:|
| Inverter run state | 0 | 1 | enum |
| PV total power | 1 | 2 | /10 W |
| PV1 voltage/current/power | 3 / 4 / 5 | 1 / 1 / 2 | /10 |
| PV2 voltage/current/power | 7 / 8 / 9 | 1 / 1 / 2 | /10 |
| PV3 voltage/current/power | 11 / 12 / 13 | 1 / 1 / 2 | /10 |
| Output power | 35 | 2 | /10 W |
| Grid frequency | 37 | 1 | /100 Hz |
| AC phase 1 V / A / W | 38 / 39 / 40 | 1 / 1 / 2 | /10 |
| AC phase 2 V / A / W | 42 / 43 / 44 | 1 / 1 / 2 | /10 |
| AC phase 3 V / A / W | 46 / 47 / 48 | 1 / 1 / 2 | /10 |
| Energy today / total | 53 / 55 | 2 / 2 | /10 kWh |
| Inverter temperatures | 93 / 94 / 95 | 1 each | /10 °C |
| Main / sub fault | 105 / 107 | 1 each | raw |

## Battery input registers (FC4)

| Entity | Address | Words | Scale |
|---|---:|---:|---:|
| Battery energy discharged today / total | 3125 / 3127 | 2 / 2 | /10 kWh |
| Battery energy charged today / total | 3129 / 3131 | 2 / 2 | /10 kWh |
| Battery derating mode | 3165 | 1 | enum |
| Battery mode/state | 3166 | 1 | packed enum |
| Battery fault / warning | 3167 / 3168 | 1 / 1 | raw |
| Battery voltage | 3169 | 1 | /10 V |
| Battery current | 3170 | 1 | /10 A |
| Battery SOC | 3171 | 1 | % |
| Battery discharge / charge power | 3178 / 3180 | 2 / 2 | /10 W |

## Writable holding registers (FC3 read / FC6 write)

| Setting | Address |
|---|---:|
| Battery discharge rate | 3036 |
| Battery SOC off-grid discharge limit | 3037 |
| Battery charge rate | 3047 |
| Battery charge SOC limit | 3048 |
| Enable AC charging | 3049 |
| Battery SOC on-grid discharge limit | 3067 |

## Time windows

Each time window is a packed 32-bit value stored in two holding registers (high word first).

| Period | High | Low |
|---:|---:|---:|
| 1 | 3038 | 3039 |
| 2 | 3040 | 3041 |
| 3 | 3042 | 3043 |
| 4 | 3044 | 3045 |
| 5 | 3050 | 3051 |
| 6 | 3052 | 3053 |
| 7 | 3054 | 3055 |
| 8 | 3056 | 3057 |
| 9 | 3058 | 3059 |

Bit layout:

```text
31       enable
30..29   priority: 0=load, 1=battery, 2=grid
28..24   start hour
23..16   start minute
12..8    end hour
7..0     end minute
```

## Growatt Smart Meter (custom FC32 / function 0x20)

The package requests 56 registers starting at address 2. All listed values use two registers (32 bit, high word first) and scale `/10`.

| Entity | Address |
|---|---:|
| Phase 1 / 2 / 3 voltage | 2 / 4 / 6 |
| Phase 1 / 2 / 3 current | 8 / 10 / 12 |
| Phase 1 / 2 / 3 active power | 14 / 16 / 18 |
| Total grid power (import/export) | 38 |
| Imported energy total | 54 |
| Exported energy total | 56 |
