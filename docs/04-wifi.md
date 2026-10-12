---
title: Wi-Fi behavior
order: 4
summary: How the setup network, saved networks, roaming and recovery work.
---

# Wi-Fi behavior

## The three situations

| Situation | What the board does |
|---|---|
| No network saved | Offers the setup network immediately and scans for networks. |
| Networks saved and one is reachable | Joins the strongest. The setup network is not offered. |
| Networks saved but none reachable for 45 seconds | Brings the setup network back while it keeps trying the saved ones. The setup network goes away again as soon as one connects. |

The third case covers moving house, a new router, or an outage: you can add the new network from your phone without touching the board.

## The setup network

- Name: `mouse-jiggler-` plus the last six characters of the board's MAC address. The OLED shows the same six characters.
- Open by default, so a phone joins it with one tap. Set `setup_password` (8 characters or more) in `jiggler.yaml` to protect it.
- Address: `192.168.4.1`.
- Phones open the setup page by themselves through the usual captive-portal detection. Any page the phone requests while the setup network is up receives the setup page, except firmware uploads.
- The LED blinks slowly while the setup network is up.

## Saved networks

- Up to five, stored in the board's flash.
- Joining a network that is already saved replaces its password.
- While not connected, a newly added network is tried immediately. If more than one network is saved, the board restarts about 20 seconds after connecting so that the full list is loaded into the connection logic.
- Adding a network while already connected, removing one, or forgetting all networks restarts the board shortly afterwards. Pairing and settings are kept.
- Wi-Fi passwords are stored unencrypted in flash, as with any ESPHome device that saves a network at runtime.

## Choosing between networks

The board always scans before it connects (`fast_connect` is off), so the strongest saved network in range wins. If the same name is offered by several access points, it picks the strongest.

## Roaming between access points

Roaming applies to access points that share one network name, such as a mesh system or several routers set up with the same name and password.

- ESPHome's built-in roaming checks five minutes after connecting, and again every five minutes, up to three times per connection.
- It only moves if another access point of the same network is at least 10 dB stronger, which stops the board bouncing between two similar ones.
- The check is skipped while a nudge is being sent, so a scan never interrupts a cursor movement.
- The Bluetooth link stays up through a roam.

## Weak-signal recovery

ESPHome stops looking for a better access point after its three checks. If the board is still stuck on a weak signal, a safety net steps in:

- If the signal stays worse than -78 dBm for 10 continuous minutes, the board drops its Wi-Fi link once.
- The normal reconnect logic then scans again and prefers a different access point.
- This happens at most once every 30 minutes, and never during a nudge, a roam or a firmware update.
- Bluetooth keeps running throughout.

This can be turned off with `weak_signal_recovery: false` in `jiggler.yaml`.

## Power saving and Bluetooth

Wi-Fi and Bluetooth share one radio on the ESP32-C3. The configuration uses light Wi-Fi power saving to leave airtime for Bluetooth. Keep the board within reasonable range of the router and avoid a crowded 2.4 GHz channel if the Bluetooth link drops.

## The board never reboots because of Wi-Fi

`reboot_timeout` is set to 0, so a router outage does not cause restarts. The board keeps jiggling, and keeps looking for the network.

## Not supported

- 5 GHz networks. The ESP32-C3 is 2.4 GHz only. Mesh systems that use one name for both bands work, because the board sees the 2.4 GHz side.
- Networks that need a username and password (WPA2-Enterprise).
- Captive portals that need a web sign-in (hotel or airport Wi-Fi).
- Static IP addresses from the page. (Edit `jiggler.yaml` and rebuild if you need one.)
