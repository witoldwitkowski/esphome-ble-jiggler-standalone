---
title: Using the web page
order: 3
summary: Every section of the control page, what it does and what the buttons change.
---

# Using the web page

The page is one file stored inside the board. It works offline, uses no external fonts or scripts, and is designed for a phone held in one hand. It refreshes itself every 1.5 seconds.

Open it at `http://mouse-jiggler-xxxxxx.local` (your six characters), or at the numeric address shown on the OLED. While the setup network is up, it is also at `http://192.168.4.1`.

## The header

At the top is a live copy of the board's 72 x 40 pixel OLED, drawn with the same shapes the firmware draws, so you can see what the board shows without looking at it. Under it, one sentence says what is going on, for example "Keeping this computer awake" or "Not connected to a computer".

## Keep awake

The master switch. Off means no cursor movement at all. The state is stored and restored after a power cut. When on and connected, the page shows when the next nudge happens and how many have been sent.

Nudge now sends one nudge immediately, which is useful for checking that pairing works.

## Computer

Shows whether a computer is paired and connected.

- **Pair with a computer** opens pairing mode for 2 minutes. Pairing is only accepted in pairing mode.
- **Stop pairing** closes it early.
- **Forget this computer** removes the stored pairing. It asks you to press a second time to confirm. Also remove the jiggler from the computer's Bluetooth list before pairing again.

Only one computer is remembered. Pairing a new one replaces the old one.

## Timing

- **Nudge every**, with the sliders **Wait at least** and **Wait at most**: the board picks a random wait between the two for every nudge. The sliders snap to 10, 15, 20, 30, 45 seconds, 1, 1.5, 2, 2.5, 3, 4, 5, 7, 10, 15, 20, 30 minutes and 1 hour. If the minimum is set above the maximum, the maximum is raised to match.
- **Nudge size**: how many pixels the cursor moves, 1 to 5. Each nudge moves out and straight back, so the cursor ends where it started.

Changes apply at once and are stored.

## Wi-Fi

- **Connected to**: the current network, signal strength and channel.
- **Saved networks**: up to five. Remove one with the Remove button (confirm by pressing again).
- **Nearby networks** (the Add a network button): the networks the board can hear, strongest first, with a lock for protected ones. Tap one to fill in the join form.
- **Other network**: type a name by hand, tick the box for a hidden network.
- **Join** (or Join this network): saves the network. The page shows the result and tells you when your phone needs to switch networks.

On first run (nothing saved, not connected) this section moves to the top and the nearby list opens by itself.

The scan list is the board's most recent scan. Search again starts a fresh scan only when no network is saved and the board is not connected (first-time setup). In every other case it re-reads the latest results, because the board's own connection logic is already scanning on a schedule and starting a second scan could drop the connection.

## Shows up as

Choose the identity the computer sees: a preset, or Something else with your own name, maker and model. Applying a new identity restarts the board and forgets the paired computer, because the old pairing belongs to the old identity. See [Choosing the mouse](./06-mouse-identity.md).

## Device

- **Screen**: a switch that turns the OLED off entirely.
- **Dim the screen after**: never, 15 seconds, 30 seconds, 1 minute or 5 minutes of inactivity.
- **Restart by itself**: never, every day, every 3 days or every week. The board only restarts between nudges, and never during a firmware update.
- **Update firmware**: opens the update page. Choose a new `firmware.ota.bin` (built next to the factory image) and upload. The OLED shows progress.
- **Restart**: reboots the board. Pairing and settings are kept.
- **Erase all settings**: forgets every Wi-Fi network, the pairing and all settings, then restarts into setup mode. Asks for a second press.

The footer shows the firmware version, the ESPHome version, uptime and free memory.

## When the page cannot reach the board

If you join a network while your phone is on the setup network, the board leaves the setup network the moment it connects, so the page loses it. The page notices and tells you to switch your phone back to your own Wi-Fi and open the address again. If the board could not join (wrong password), the setup network returns after a short time and the page shows the reason.
