# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

ZMK firmware configuration for the **Cheapino v2** — a custom 36-key split keyboard using a single **nice!nano v2** controller with a charlieplex key matrix. The firmware is built by GitHub Actions using the official ZMK build workflow.

## Building

Firmware is built via GitHub Actions — push to `main` and the workflow at `.github/workflows/build.yml` triggers the ZMK build pipeline automatically. There is no local build command; all compilation happens in CI.

The build target is defined in `build.yaml`:
- Board: `nice_nano_v2`
- Shield: `cheapinov2`

## Key files

- `boards/shields/cheapinov2/cheapinov2.keymap` — the keymap (layers, bindings, behavior config). **This is where most edits happen.**
- `config/cheapinov2.conf` — top-level Kconfig flags (ZMK Studio enabled, battery reporting off)
- `boards/shields/cheapinov2/cheapinov2.conf` — shield-level Kconfig
- `boards/shields/cheapinov2/cheapinov2.dtsi` — GPIO pin assignments for the charlieplex matrix and the matrix transform
- `boards/shields/cheapinov2/cheapinov2-layout.dtsi` — physical key positions (used by ZMK Studio for visual layout)
- `config/west.yml` — pins ZMK to `zmkfirmware/zmk@main`

## Keymap architecture

The keymap (`cheapinov2.keymap`) has **7 layers** accessed via `lt` (layer-tap) thumb keys:

| Index | Name     | Left thumb activator |
|-------|----------|----------------------|
| 0     | BASE     | —                    |
| 1     | MEDIA    | `lt 1 ESC`           |
| 2     | NAV      | `lt 2 SPACE`         |
| 3     | MOUSE    | `lt 3 TAB`           |
| 4     | SYMBOL   | `lt 4 ENTER`         |
| 5     | FUNCTION | `lt 5 DELETE`        |
| 6     | NUMPAD   | `lt 6 BACKSPACE`     |

The BASE layer uses home-row mods (`&mt`) on both hands with `tap-preferred` flavor, short tapping term (150ms), and `require-prior-idle-ms` to avoid accidental mod activation during fast typing.

The physical layout is 3×5 per hand + 3 thumb keys per hand (36 keys total). The charlieplex matrix uses 12 GPIO pins on the nice!nano v2 (mapped via `pro_micro` aliases). Note: right-side rows 1 and 2 are swapped in routing — handled in the matrix transform.

## ZMK Studio

`CONFIG_ZMK_STUDIO=y` is set, so the firmware supports live keymap editing via ZMK Studio without reflashing.
