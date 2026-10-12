---
title: Development
order: 15
summary: Building, working on the page without hardware, and the project layout.
---

# Development

## Build

```
pip install esphome            # 2026.9 or newer
esphome compile jiggler.yaml   # factory image and OTA image
esphome run jiggler.yaml       # flash over USB and show logs
```

For local components, switch `external_components` in `jiggler.yaml` to the commented-out local block.

Outputs are in `.esphome/build/mouse-jiggler/build/`: `firmware.factory.bin` (first flash over USB) and `firmware.ota.bin` (updates from the page).

## Working on the page without hardware

```
python3 tools/mock_device.py setup      # first-run: no networks saved, setup network up
python3 tools/mock_device.py home       # connected, paired and on
python3 tools/mock_device.py paired     # paired but computer not connected
python3 tools/mock_device.py pairing    # pairing mode open
```

An optional second argument sets the port (default 8765). Open `http://localhost:8765`. The mock serves the real `index.html` and implements the same `/api/*` routes with enough behavior to click through every flow, including a join with a wrong and then a right password.

Reload to see edits to `index.html`; the firmware build gzips and embeds it, so rebuild when you are done.

## Design rules for the page

- One file, no external requests (it must work on the setup network with no internet).
- System fonts only.
- Phone first; usable at 320 pixels wide.
- No `alert()` or `confirm()`: destructive actions use an inline second press.
- Respect `prefers-reduced-motion` and `prefers-color-scheme`.

## Scheduler simulation

```
python3 tools/simulate_jitter.py
```

Models the jiggle scheduler and asserts its invariants. It does not test Bluetooth.

## Where things are

| Path | Purpose |
|---|---|
| `jiggler.yaml` | Firmware configuration |
| `components/ble_hid_mouse/` | Bluetooth HID mouse, jiggle engine, OLED drawing (`jiggler_ui.h`) |
| `components/jiggler_portal/__init__.py` | ESPHome schema and code generation; gzips and embeds the page |
| `components/jiggler_portal/jiggler_portal.cpp` / `.h` | Web handler, API, storage, identities, watchdog |
| `components/jiggler_portal/web/index.html` | The page |
| `tools/` | Mock device and scheduler simulation |
| `docs/` | This documentation |

## Adding a mouse preset

Add a row to the `PROFILES` table in `jiggler_portal.cpp` (label, name, maker, model, ID source, vendor ID, product ID, version) and to the list in `tools/mock_device.py`. The page reads the list from `/api/profiles`.

## Relationship to the Home Assistant edition

This project began as the Home Assistant edition (`esphome-ble-jiggler`) and shares its Bluetooth component. The copy here has small additions: a way to clear bonds, a GAP name call that matches the chosen identity, and the standalone screens in `jiggler_ui.h`. The two copies are separate; changes to one do not reach the other automatically.
