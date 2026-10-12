---
title: Security and privacy
order: 11
summary: What the board exposes, who can reach it, and what the names do and do not do.
---

# Security and privacy

This is a desk gadget, and it is built like one. Read this before putting it somewhere you care about.

## The setup network is open by default

An open network is easiest on a phone. It only exists while no saved Wi-Fi works, and anyone within range could join it during that time and change settings. If that matters, set `setup_password` (8 characters or more) in `jiggler.yaml`.

## The page has no login

Anyone on your Wi-Fi can open the page and use it, including switching the jiggler off, forgetting the paired computer, erasing settings or uploading firmware. That is fine for a home network you trust. Do not forward port 80 to the internet, and think twice on shared or guest networks.

## Wi-Fi passwords are stored in plain text

Up to five passwords are kept in the board's flash without encryption, as with any ESPHome device that saves a network at runtime. Anyone with physical access and a USB cable can read them. Use the Erase all settings button, or the 20-second BOOT hold, before giving the board away.

## Traffic is not encrypted

The page uses plain HTTP. Wi-Fi passwords you type into the setup page cross your own setup network in the clear. Joining happens at close range, but be aware of it.

## The Bluetooth identity is a label

The name, maker and IDs are what a computer shows in its Bluetooth list. They do not hide the device, and they do not make it behave like the product it is named after.

Managed computers can log and restrict Bluetooth devices. Nothing here makes the jiggler invisible to IT. Your employer's or school's policies on the equipment you use are yours to follow.

## The Bluetooth side

- Pairing is only accepted while pairing mode is open (2 minutes).
- Only one computer is remembered, and a new pairing replaces it.
- The HID characteristics require an encrypted link.
- "Just Works" pairing has no passkey, so anyone nearby could pair during the pairing window. Open it only when you are ready.
- The device is a mouse only: it sends relative movement and never a button, wheel or key.

## Updates

Firmware uploads go through the page with no password, like the rest of the page. This edition does not add one.
