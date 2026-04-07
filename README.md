# Cheapino v2 ZMK Config

ZMK firmware for the [Cheapino v2](https://github.com/tompi/cheapino) — a 36-key split keyboard.

## Current builds

| Board | Shield | Output |
|-------|--------|--------|
| nice!nano v2 (`nice_nano/nrf52840/zmk`) | `cheapinov2` | BLE + USB, `.hex` |
| RP2040 Zero (`rp2040_zero/rp2040/zmk`) | `cheapinov2` | USB, `.uf2` |

Push to `main` → GitHub Actions builds both and produces artifacts.

## Planned: Wireless split

A future variant with two independent halves and a USB dongle:

- **Left / Right halves** — nice!nano v2 clones, BLE peripherals, OLED display, LiPo battery
- **Dongle** — nice!nano v2 clone or nRF52840 USB-A dongle, BLE central → USB HID to PC

**Rough phases:**
1. Scaffold ZMK split config (left, right, dongle overlays)
2. Single half over USB — verify key scan and interrupt wake
3. Add OLED display
4. Add battery
5. Repeat for second half
6. Wire up wireless split with dongle
7. Flash production nRF52840 USB-A dongle
8. Polish — power tuning, display widgets, keymap

**Risk items to resolve early:** charlieplex vs matrix driver for duplex wiring; ZMK deep-sleep wake bug on main branch; nice!nano clone quiescent current.
