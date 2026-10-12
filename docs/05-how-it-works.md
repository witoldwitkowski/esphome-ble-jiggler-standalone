---
title: How it works
order: 5
summary: Architecture, the two components, storage, the request flow and the Bluetooth side.
---

# How it works

## The pieces

```
 phone / laptop ──HTTP──▶ ESP32-C3 ──Bluetooth LE──▶ computer
                            │
                            ├─ web server (the page + /api/*)
                            ├─ Wi-Fi: setup network + saved networks
                            ├─ jiggle scheduler
                            └─ OLED, LED, BOOT button
```

The firmware is an ESPHome configuration (`jiggler.yaml`) plus two custom components:

| Component | Job |
|---|---|
| `ble_hid_mouse` | Bluetooth LE HID-over-GATT mouse: services, pairing, bonding, advertising, the jiggle engine, and the OLED drawing code. |
| `jiggler_portal` | The web page and its JSON API, stored settings and Wi-Fi networks, mouse identity presets, the weak-signal watchdog, optional scheduled restart. |

ESPHome's stock `wifi`, `captive_portal`, `web_server_base`, `ota` (web_server platform) and `mdns` components do the rest.

## The Bluetooth mouse

`ble_hid_mouse` registers its own GATT application on the same Bluedroid stack ESPHome's `esp32_ble` owns, so Wi-Fi, OTA and the web server run unmodified beside it. It builds three services: HID, Battery and Device Information.

- The HID report map describes a standard three-button mouse with relative X and Y. The firmware only ever sends zero buttons and zero wheel.
- All HID characteristics require an encrypted link. Pairing uses "Just Works" (no passkey), like a real mouse.
- **Single host policy**: a new pairing is only accepted while pairing mode is open, and accepting one erases any previous bond.
- **Advertising**: during pairing mode it advertises openly for the pairing timeout (default 120 s). When bonded but disconnected it advertises fast for 30 s after the disconnect, then slowly. With no bond and no pairing mode it does not advertise at all.
- **Ready** means: link up, encrypted, and the computer has subscribed to input reports. That is the only state in which a nudge can be delivered.
- The Device Information service reports the manufacturer, model and a PnP ID (vendor and product ID) taken from the chosen identity. The GAP device name is set to match.

### The jiggle engine

- After switching on (and after every nudge), the next nudge is scheduled at a random time between the minimum and maximum wait.
- A nudge moves the cursor a random distance of 1 up to the chosen size, in one of four directions (up, down, left, right), in small steps 12 to 24 ms apart. After a pause of 30 to 120 ms it moves back by the same amount, so the net movement is always zero.
- Nothing is sent while the computer is disconnected, and missed nudges are not queued or replayed. If a disconnect interrupts a nudge, the move is undone on reconnect.
- Wait range: 10 to 3600 seconds. Size setting: 1 to 5 pixels.

## The portal

`jiggler_portal` is an ESPHome `AsyncWebHandler` registered ahead of the captive portal's, so it answers first.

- **Claims**: every `/api/*` request, `GET /` and `/index.html`. While the setup network is active it also claims every other GET except `/update*`, so phones' sign-in probes receive the page.
- **State snapshot**: `GET /api/state` returns a JSON snapshot that the main loop rebuilds once per second under a mutex, so web-server threads never touch live objects.
- **Mutations**: POST handlers validate their input synchronously (so the page can show a reason) and then `defer()` the change to the main loop.
- **The page** is a single HTML file, gzip-compressed at build time and embedded in flash. It polls the state every 1.5 seconds.

### Startup order

The portal's setup runs before Wi-Fi's (so saved networks are loaded into the Wi-Fi component first) and before the Bluetooth mouse's (so the chosen identity is in place before the GATT table is built, because the table points at those strings).

## Stored data

Settings use ESPHome's preference store (NVS flash). Two blobs:

| Key | Contents |
|---|---|
| `0x4A494701` | Settings: identity choice, nudge size, wait range, on/off, screen off, dim time, restart days, custom name, maker and model. |
| `0x4A494702` | Up to five Wi-Fi networks (name, password, hidden flag). |

Each blob starts with a magic byte and is range-checked on load. A bad or missing blob falls back to the defaults. Bluetooth bonds are stored separately by the Bluetooth stack. Normal restarts and firmware updates keep all of it.

## Wi-Fi

ESPHome's wifi component supports one saved network, so the portal keeps its own list and hands ESPHome the networks at boot (or live, when adding one while in setup mode). See [Wi-Fi behavior](./04-wifi.md) for the rules.

## The screen

The OLED is drawn from shapes and a tiny built-in font (no image files). The normal screen shows the Bluetooth state, whether the jiggler is on, Wi-Fi bars and progress to the next nudge. Three full-screen displays take over when needed: the setup screen (broadcast arcs and the six-character tag), the address screen (the IP in large digits) and the firmware-update bar. [Button, LED and screen](./07-button-and-display.md) lists them all.

The web page's header draws the same screens in a canvas, from the same state.

## Source layout

```
jiggler.yaml                         firmware configuration
components/ble_hid_mouse/            Bluetooth mouse, jiggle engine, OLED drawing
components/jiggler_portal/           web handler, storage, identity presets, watchdog
components/jiggler_portal/web/       the page (index.html, one file)
tools/mock_device.py                 fake device for developing the page in a browser
tools/simulate_jitter.py             scheduler simulation with assertions
docs/                                this documentation
```
