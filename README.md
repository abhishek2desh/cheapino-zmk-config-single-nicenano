# Cheapino v2 ZMK Config

[![.github/workflows/build.yml](https://github.com/oslundstrom/cheapino-zmk-config-single-nicenano/actions/workflows/build.yml/badge.svg)](https://github.com/oslundstrom/cheapino-zmk-config-single-nicenano/actions/workflows/build.yml)

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

**Remaining phases:**
1. Fix keymap / matrix pin mapping for left half
2. Add OLED display
3. Repeat for second half
4. Wire up full wireless split with dongle
5. Flash production nRF52840 USB-A dongle
6. Polish — power tuning, display widgets, keymap

**Risk items to resolve early:** charlieplex vs matrix driver for duplex wiring; ZMK deep-sleep wake bug on main branch; nice!nano clone quiescent current.

## Credits

- **[tompi](https://github.com/tompi)** — original ZMK config this repo is forked from ([tompi/cheapino-zmk-config-single-nicenano](https://github.com/tompi/cheapino-zmk-config-single-nicenano)), and designer of the [Cheapino v2](https://github.com/tompi/cheapino) keyboard hardware (CC-BY-4.0).
- **[ZMK Firmware](https://github.com/zmkfirmware/zmk)** — the open-source keyboard firmware this config targets.

## License

[MIT](LICENSE) © Oskar Lundstrom
