---
title: Getting started
order: 2
summary: Flash the board once, join its setup network, choose your Wi-Fi, pair.
---

# Getting started

Allow about ten minutes. You need the board, a USB-C cable, a computer with Chrome or Edge for the one-time flash, and your phone.

## 1. Build the firmware

Install ESPHome 2026.9 or newer, then compile the configuration:

```
pip install esphome
git clone https://github.com/witoldwitkowski/esphome-ble-jiggler-standalone
cd esphome-ble-jiggler-standalone
esphome compile jiggler.yaml
```

The first build downloads the toolchain and takes several minutes. The file you need afterwards is:

```
.esphome/build/mouse-jiggler/build/firmware.factory.bin
```

To change defaults first, such as the pins or the starting timing, edit the `substitutions:` block at the top of `jiggler.yaml`. See [Configuration](./09-configuration.md).

## 2. Flash it once over USB

1. Open <https://web.esphome.io> in Chrome or Edge.
2. Plug the board in, click Connect, and pick its serial port.
3. Click Install and choose `firmware.factory.bin`.

If the upload will not start, put the board in download mode: hold BOOT, tap RST, release BOOT. After this first flash, updates can be done from the web page.

## 3. Join the setup network

The board now offers its own Wi-Fi network named `mouse-jiggler-` followed by six characters. The same six characters appear on the OLED under a broadcasting symbol, so you can tell several boards apart.

On your phone, join that network. It is open (no password) unless you set `setup_password`. The setup page should open by itself, the same way a hotel Wi-Fi sign-in page does. If it does not, open `http://192.168.4.1` in the browser.

## 4. Choose your Wi-Fi

The list of nearby networks opens first. Tap yours, type the password and press Join. The board connects, and the page tells you to switch your phone back to your own Wi-Fi.

For a hidden network, use Other network and tick the hidden box. Networks that need a username are not supported.

## 5. Open the page on your home network

With your phone back on your home Wi-Fi, open:

```
http://mouse-jiggler-xxxxxx.local
```

with your six characters. The numeric address is also on the OLED for 20 seconds after the board connects, and any time you tap BOOT. Bookmark it. Some Android versions cannot open `.local` names; use the numeric address there.

## 6. Pair with the computer

1. On the page, press Pair with a computer. The OLED shows the Bluetooth symbol with animated waves, and pairing mode stays open for 2 minutes.
2. On a Mac: System Settings, Bluetooth, then Connect next to the mouse name. On Windows: Settings, Bluetooth & devices, Add device, Bluetooth. On Linux: your Bluetooth settings.
3. The page headline changes when the computer is connected.

## 7. Switch Keep awake on

That is it. The setting is stored, so the jiggler resumes by itself after a power cut.

## Pairing without Wi-Fi

You can skip steps 3 to 5. Hold BOOT for 3 seconds to enter pairing mode, pair from the computer, and the jiggler runs with its stored settings (the jiggler starts switched off, and the only way to switch it on is the page, so you need the page at least once).
