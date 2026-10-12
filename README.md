# Mouse Jiggler: phone-configured BLE mouse (ESPHome, ESP32-C3)

A small ESP32-C3 board that pairs with your computer as a Bluetooth mouse and nudges the cursor now and then so it never idles. Set it up from your phone: no Home Assistant, no router changes, no app.

- **Its own setup Wi-Fi** on first boot. Join it, pick your home Wi-Fi from a list, done.
- **Remembers five networks**, joins the strongest, roams between access points, recovers from a weak signal.
- **Pick the mouse** it pretends to be: ten presets or your own name.
- **A web page** for pairing, the on/off switch, timing, Wi-Fi, firmware updates. It shows a live copy of the board's OLED.
- **Works without Wi-Fi**; settings survive power cuts.
- **Mouse only**: it never clicks, scrolls or types.

## Quick start

```
pip install esphome
git clone https://github.com/witoldwitkowski/esphome-ble-jiggler-standalone
cd esphome-ble-jiggler-standalone
esphome compile jiggler.yaml
```

Flash `.esphome/build/mouse-jiggler/build/firmware.factory.bin` once over USB with <https://web.esphome.io>, join the `mouse-jiggler-xxxxxx` network on your phone, and follow the page. Full steps: [Getting started](docs/02-getting-started.md).

## Documentation

See [docs/](docs/README.md): [overview](docs/01-overview.md), [using the web page](docs/03-web-page.md), [Wi-Fi behavior](docs/04-wifi.md), [how it works](docs/05-how-it-works.md), [HTTP API](docs/08-api.md), [configuration](docs/09-configuration.md), [security](docs/11-security.md), [troubleshooting](docs/12-troubleshooting.md).

## Status

Compiles cleanly, and the page was tested in a browser against a mock of the device. **Not yet run on a physical board**: the setup network and phone popup, joining a real router, roaming, pairing with real computers and the OLED screens are unverified. Details in [Status and testing](docs/13-status.md).

## Security in one paragraph

The setup network is open by default (set `setup_password` to change that), and the page has no login on your home network. Wi-Fi passwords are stored unencrypted in flash. The mouse name and IDs are labels, not disguise. Read [Security and privacy](docs/11-security.md).

## Using Home Assistant?

The Home Assistant edition, with entities, dashboard card and automations, is [esphome-ble-jiggler](https://github.com/witoldwitkowski/esphome-ble-jiggler).

## Layout

```
jiggler.yaml                      firmware configuration
components/ble_hid_mouse/         Bluetooth mouse, jiggle engine, OLED drawing
components/jiggler_portal/        web page, API, stored settings, identities
tools/                            mock device for the page, scheduler simulation
docs/                             documentation
```
