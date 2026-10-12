---
title: Troubleshooting
order: 12
summary: Symptoms, causes and fixes.
---

# Troubleshooting

## The setup network does not appear

- It only appears when no saved network works. If the board is already on your Wi-Fi, it is not offered.
- Check the LED: a slow blink means the setup network is up.
- The OLED should show broadcast arcs and six characters.
- Wait up to a minute after power-up. The network can take a moment, and a board with saved networks waits 45 seconds before offering it.
- Force it: hold BOOT for 20 seconds to forget all Wi-Fi networks and restart.
- The ESP32-C3 is 2.4 GHz only. Some phones prefer to stay on a 5 GHz network they were already on; switch Wi-Fi manually.

## The setup page does not open by itself

Open `http://192.168.4.1` in the browser. Some phones keep mobile data on and refuse to use a network without internet; turn mobile data off for the setup, or accept the "stay connected" prompt.

## Joining my Wi-Fi fails

- Re-check the password (use Show). Wi-Fi passwords are case-sensitive.
- The network must be 2.4 GHz and use WPA2 or WPA3 personal, or be open.
- Hidden networks need the hidden box ticked.
- After a failed attempt the setup network returns after a short time, and the page tells you what happened.
- Move the board closer to the router for setup.

## I joined but cannot find the page

- Switch your phone back to your home Wi-Fi first.
- Use the address on the OLED (tap BOOT to see it again) or `http://mouse-jiggler-xxxxxx.local`.
- Android versions that cannot resolve `.local` need the numeric address.
- Your router may block device-to-device traffic (guest networks, "client isolation"). Use the normal network.

## The computer does not see the mouse

- Pairing must be open when you click Connect: press Pair with a computer on the page, or hold BOOT for 3 seconds. The OLED shows waves.
- Look for the name shown under Shows up as, not for "Mouse Jiggler".
- Forget the device on **both** sides: remove it from the computer's Bluetooth list, and press Forget this computer on the page. Then pair again.
- A mouse change on the page forgets the pairing on the board, but not on the computer.

## Paired, but the page says "not connected to a computer"

The computer must connect and subscribe to the mouse reports. If the OLED shows a solid Bluetooth symbol but nothing moves, forget on both sides and pair again. If the computer sleeps, the link drops and the board keeps advertising; it reconnects on wake. No nudges are queued while it is away.

## The cursor does not move

- Keep awake must be on (the switch on the page, and the mouse icon on the OLED filled).
- The computer must be connected: the headline says so.
- Press Nudge now to test.
- At the smallest size (1 pixel) a nudge is hard to see. Try 3.

## Bluetooth keeps dropping

Wi-Fi and Bluetooth share the radio. Move the board closer to the router, avoid a crowded 2.4 GHz channel, and keep the power-save setting at `light`.

## The page stops updating

The page polls every 1.5 seconds. If it shows an old state, the board may have restarted (after a mouse change, a restart or an update). It reconnects by itself. If the board moved to another Wi-Fi network, its address may have changed.

## The screen is blank

- The screen switch on the page may be off.
- Run the display bring-up in [Hardware](./10-hardware.md).

## Firmware update fails

Make sure you chose `firmware.ota.bin`, not the factory image. Keep the board close to the router during the upload. If the update page does not open, hold BOOT for 20 seconds (this also clears Wi-Fi), or re-flash over USB.

## I am locked out

Hold BOOT for 20 seconds. This forgets every Wi-Fi network and restarts into setup mode. The Erase all settings button on the page does the same, and also clears settings and the pairing.

## Logs

Over USB: `esphome logs jiggler.yaml`. Look for lines tagged `ble_hid_mouse` and `jiggler_portal`.
