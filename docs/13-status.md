---
title: Status and testing
order: 13
summary: What has been checked, what has not, and how to help close the gaps.
---

# Status and testing

This edition was built and tested without access to the physical board. Be clear about what that means.

## Checked

| Item | Result |
|---|---|
| `esphome config` and `esphome compile` | Pass with no warnings. Flash 69.9% of one OTA slot, RAM 42.6%. |
| The Home Assistant edition, rebuilt with the same Bluetooth component | Unchanged build. |
| The web page, in Chrome at phone width, against a mock of the API (`tools/mock_device.py`) | Works: first-run network list, joining with a wrong and then the right password, the "your phone left the setup network" message, pairing, forgetting, the switch, changing the mouse, timing. |
| The jiggle scheduler (`tools/simulate_jitter.py`) | Passes: intervals in range, net movement always zero, nothing sent while disconnected, no replay. It models the scheduler only, not Bluetooth. |

## Not checked on real hardware

| Item | Why it matters |
|---|---|
| The setup network appearing and the phone's "sign in" popup opening the page | The core first-run experience, on iOS and on Android. |
| Joining a real router and reconnecting after a power cut | Storage and the reconnect path. |
| Several saved networks, roaming between access points, the weak-signal watchdog | Logic is ESPHome's plus one small watchdog; never run against a real mesh. |
| Pairing with a Mac, Windows PC or Linux machine under the new presets | Identity strings and the GAP name. |
| The OLED setup and address screens | Drawn from shapes; never seen on a panel. |
| The pins and display offsets for a specific board | Defaults only. |
| Long-running Wi-Fi plus Bluetooth stability | Shared radio. |
| That the web handler really answers before the captive portal's, and the web server starting correctly in setup mode | Designed from the ESPHome source; not exercised on a device. |
| Fetching the components from GitHub | Needs the repository to be published (see [Configuration](./09-configuration.md)). |

## Known limits

- Up to five Wi-Fi networks. No WPA2-Enterprise, no captive-portal sign-ins, 2.4 GHz only.
- One paired computer at a time.
- No page login.
- The Microsoft, Dell, HP and Lenovo presets only change the name and maker.
- While connected, the page shows the last scan, not a fresh one.
- A static IP needs a rebuild.

## Helping test

If you try it, the most useful reports are:

1. Which phone and OS version, and whether the setup page opened by itself.
2. Which computer and OS, and what the Bluetooth list shows.
3. The log from `esphome logs jiggler.yaml` around any problem.
4. A photo of the OLED if it looks wrong.
