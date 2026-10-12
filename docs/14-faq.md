---
title: FAQ
order: 14
summary: Short answers to the questions people ask first.
---

# FAQ

**Do I need Home Assistant?** No. That is the point of this edition. A separate edition exists for people who do use Home Assistant, in the repository `esphome-ble-jiggler`.

**Do I need the internet?** Only to build the firmware. The page and the setup flow work with no internet.

**Does it work without Wi-Fi?** Yes. Pair with the BOOT button (hold 3 seconds). Switching it on needs the page once.

**Can it pair with two computers?** No. One at a time. A new pairing replaces the old one.

**Will it work with a Mac, Windows PC, Linux, iPad?** It is a standard Bluetooth LE HID mouse, which these systems support. Pairing has not yet been verified against real machines (see [Status](./13-status.md)).

**Why does changing the mouse restart it and forget the computer?** The computer remembers a device by its identity. Changing the identity makes the old pairing invalid, so the board clears it and restarts, and you pair again.

**How big is a nudge?** One to five pixels (you choose), out and straight back, in one of four directions. The cursor ends where it started.

**Can it press keys or click?** No. Reports never include buttons, wheel or keys.

**How often does it nudge?** A random time between your minimum and maximum, from 10 seconds to 1 hour.

**Does it stop if the computer is asleep?** It sends nothing while the link is down and does not catch up afterwards. It reconnects when the computer wakes.

**Can I use a 5 GHz network?** No. The board is 2.4 GHz only. Mesh systems that use one name for both bands are fine.

**Can I use a hotel or office Wi-Fi?** Not if it needs a web sign-in or a username.

**I have three routers with the same Wi-Fi name. Will it move between them?** Yes, when another one is clearly stronger. See [Wi-Fi behavior](./04-wifi.md).

**What if I move house?** The setup network comes back 45 seconds after the saved networks stop working. Add the new network from your phone.

**How do I update it?** Page, Device, Update firmware, with `firmware.ota.bin`.

**Where are the settings stored?** In the board's flash. They survive power cuts, restarts and updates.

**Is this safe to leave on my network?** It has no login. See [Security](./11-security.md).

**Can I change the page?** Yes: edit `components/jiggler_portal/web/index.html`, test it with `tools/mock_device.py`, rebuild. See [Development](./15-development.md).

**What license is it under?** None has been chosen yet. Until one is added, the default is all rights reserved.
