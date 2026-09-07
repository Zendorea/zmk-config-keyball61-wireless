# zmk-config-keyball61-wireless

Wireless (ZMK) firmware for a **Keyball61** (holykeebs Keyball61 **rev2**) running
on **nice!nano v2 (nRF52840)** per half, with a **PMW3610** optical trackball on
the **right/central** half — the custom PMW3610 daughterboard from
[Zendorea/keyball @ feat/pmw3610-wireless](https://github.com/Zendorea/keyball/tree/feat/pmw3610-wireless)
(`keyball61_wireless/pmw3610_daughterboard`).

## Provenance

This config is based **unchanged** on the proven, tested
[tangbonze/zmk-config-Keyball61](https://github.com/tangbonze/zmk-config-Keyball61)
(nice!nano v2 + PMW3610, ZMK v0.3). holykeebs replicates the Yowkees Keyball61
electrically, so this config's wiring matches the holykeebs rev2 board. The
tangbonze `zmk-pmw3610-driver` fork + its `CONFIG_PMW3610_*` tuning knobs +
keymap are a coherent, working set — kept intact rather than swapped, to
minimise risk on a first wireless build.

## Verified wiring (right/central half)

| Signal | nice!nano pin | Daughterboard |
|--------|---------------|---------------|
| SPI1 SCK  | P1.13 | CK |
| SPI1 SDIO (MOSI+MISO, 3-wire) | P0.10 | IO |
| CS / NCS  | P0.09 | CS |
| MOTION / IRQ | P1.11 (active-low, pull-up) | MOT |
| 3V3 / GND | — | V / G |
| NRESET | — (SPI soft-reset; tied via R2) | RST |

Matrix (right): rows `pro_micro 3–7`, cols `9,8,18,19,20,21`, `col-offset 7`.
OLED SSD1306 on i2c0 (SDA P0.17, SCL P0.08). Split over BLE (TRRS serial unused).

> ⚠️ **Verify against your physical rev2 PCB before flashing:** confirm the
> trackball connector maps to CS=P0.09 / IRQ=P1.11 as above. Research indicates
> rev1≈rev2 wiring but this was not independently confirmed for rev2 silk.

## Sensor note

The stock holykeebs Keyball uses a **PMW3360** (QMK). This build swaps in the
**PMW3610** (lower power, better for wireless) via the daughterboard + the
`CONFIG_PMW3610_*` driver here. Default sensor orientation is
`CONFIG_PMW3610_ORIENTATION_180` — adjust if your mount differs.

## Build

Push to GitHub; `.github/workflows/build.yml` builds per `build.yaml`:
- `keyball61_left`  → nice_nano_v2
- `keyball61_right` → nice_nano_v2 (+ studio-rpc-usb-uart, trackball)
- `settings_reset`  → nice_nano_v2

Download the artifacts, flash `*_left.uf2` to the left nice!nano and
`*_right.uf2` to the right (double-tap reset → drag onto the `NICENANO` drive).
Flash `settings_reset.uf2` to either half first if you need to clear old BLE bonds.

## Credits
- Base config: https://github.com/tangbonze/zmk-config-Keyball61
- Driver: https://github.com/tangbonze/zmk-pmw3610-driver
- Alt (future / ZMK main): https://github.com/badjeff/zmk-pmw3610-driver
