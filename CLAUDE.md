# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

ZMK firmware configuration for the **Cheapino v2** — a custom 36-key single-PCB keyboard. Two MCU targets are supported: **nice!nano v2** (BLE + USB) and **RP2040 Zero** (USB only). A **wireless split variant** with two independent halves and a USB dongle is planned (see below).

## Building

Firmware is built via GitHub Actions — push to `main`. No local build. Both targets are defined in `build.yaml`:

```yaml
- board: nice_nano/nrf52840/zmk   → shield: cheapinov2
- board: rp2040_zero/rp2040/zmk  → shield: cheapinov2
```

## Shield structure

```
boards/shields/cheapinov2/
├── cheapinov2.keymap          ← all layer/binding edits go here
├── cheapinov2.dtsi            ← matrix transform + physical layout (board-agnostic)
├── cheapinov2-layout.dtsi     ← physical key positions for ZMK Studio
├── cheapinov2.conf            ← shield Kconfig (mouse, battery reporting)
├── cheapinov2.zmk.yml         ← shield metadata
└── boards/
    ├── nice_nano_nrf52840_zmk.overlay   ← &pro_micro charlieplex GPIO pins
    └── rp2040_zero_rp2040_zmk.overlay   ← &gpio0 charlieplex GPIO pins

boards/rp2040_zero/            ← ZMK board variant (not in ZMK upstream)
├── board.yml                  ← extends rp2040_zero with zmk variant
├── Kconfig.rp2040_zero        ← selects ZMK_BOARD_COMPAT
├── rp2040_zero_rp2040_zmk.dts
└── rp2040_zero_rp2040_zmk_defconfig

config/cheapinov2.conf         ← ZMK Studio + USB logging flags
zephyr/module.yml              ← board_root: . (exposes boards/ to ZMK)
```

To add a third MCU: create `boards/shields/cheapinov2/boards/<board_id>.overlay` with the kscan node.

## Keymap

7 layers accessed via `lt` thumb keys. From left to right on each thumb cluster:

| Left thumb | Layer | Right thumb | Layer |
|------------|-------|-------------|-------|
| `lt 1 ESC` | MEDIA | `lt 4 ENTER` | SYMBOL |
| `lt 2 SPACE` | NAV | `lt 6 BACKSPACE` | NUMPAD |
| `lt 3 TAB` | MOUSE | `lt 5 DELETE` | FUNCTION |

HOME-ROW MODS on BASE layer: `&mt` with `tap-preferred`, 150ms tapping term, `require-prior-idle-ms = 150`.

## Charlieplex matrix

12 GPIO pins total (6 per side). The matrix transform encodes each key as `RC(driver_pin, reader_pin)`. Pin ordering: left rows 0-2, left cols 3-5, right rows 6-8, right cols 9-11. The nice!nano PCB has right-side rows 1+2 swapped in routing — compensated by the pin order in the overlay.

## ZMK Studio

`CONFIG_ZMK_STUDIO=y` — live keymap editing without reflashing works on both targets.

---

## Wireless split variant

Target architecture: two BLE peripheral halves + one USB central dongle. Implemented in `boards/shields/cheapino_wireless/`.

**Shield files:**
```
boards/shields/cheapino_wireless/
├── cheapino_wireless_left.overlay    ← kscan + transform for left half
├── cheapino_wireless_right.overlay   ← kscan + transform for right half (pins TBD)
├── cheapino_wireless_dongle.overlay  ← mock kscan, central role, physical layout
├── Kconfig.shield
├── Kconfig.defconfig
├── cheapino_wireless_{left,right,dongle}.zmk.yml
└── boards/
    └── nice_nano_nrf52840_zmk.overlay  ← CDC ACM uart + zephyr,console for USB logging
```

**Build targets:**
```yaml
- board: nice_nano/nrf52840/zmk        shield: cheapino_wireless_left
- board: nice_nano/nrf52840/zmk        shield: cheapino_wireless_right
- board: nice_nano/nrf52840/zmk        shield: cheapino_wireless_dongle
- board: nrf52840dongle/nrf52840/zmk   shield: cheapino_wireless_dongle
```

**USB logging:** enabled only for dongle (`config/cheapino_wireless_dongle.conf`) and left half (`config/cheapino_wireless_left.conf`). The nice!nano ZMK variant lacks the `zephyr,cdc-acm-uart` DT node — added via `boards/nice_nano_nrf52840_zmk.overlay`.

### Left half GPIO pin mapping (nice!nano v2)

6 charlieplex pins in order. Pin ordering convention: Row0 (top), Row1 (middle), Row2 (bottom), Col0 (leftmost / thumb column), Col1, Col2 (rightmost).

| Index | Role | nRF52840 pin | RP2040 equiv |
|-------|------|-------------|--------------|
| 0 | Row0 | P1.11 | GP29 |
| 1 | Row1 | P1.13 | GP28 |
| 2 | Row2 | P1.15 | GP27 |
| 3 | Col0 + thumb col | P0.02 | GP26 |
| 4 | Col1 | P0.29 | GP15 |
| 5 | Col2 | P0.31 | GP14 |

### Right half GPIO pin mapping

Pins TBD — right overlay currently has placeholder pins.
