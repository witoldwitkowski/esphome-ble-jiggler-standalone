---
title: Choosing the mouse
order: 6
summary: The ten presets, custom identities, and what changing identity does.
---

# Choosing the mouse

The computer lists Bluetooth devices by name and shows the maker in details. The jiggler lets you pick what it reports, under Shows up as on the page.

## Presets

| Preset | Name shown | Maker | Vendor : Product ID |
|---|---|---|---|
| Logitech MX Anywhere 3 (default) | MX Anywhere 3 | Logitech | 046D : B025 |
| Logitech MX Master 3S | MX Master 3S | Logitech | 046D : B034 |
| Logitech MX Master 3 | MX Master 3 | Logitech | 046D : B023 |
| Logitech MX Anywhere 2S | MX Anywhere 2S | Logitech | 046D : B01A |
| Logitech M720 Triathlon | M720 Triathlon | Logitech | 046D : B015 |
| Microsoft Bluetooth Mouse | Microsoft Mouse | Microsoft | 045E : 0001 |
| Dell Wireless Mouse | Dell Wireless Mouse | Dell | 413C : 0001 |
| HP Bluetooth Mouse | HP Bluetooth Mouse | HP | 03F0 : 0001 |
| Lenovo Bluetooth Mouse | Lenovo BT Mouse | Lenovo | 17EF : 0001 |
| Generic Bluetooth mouse | Bluetooth Mouse | Generic | 1209 : 0001 |

The Logitech IDs come from public USB ID lists. The Microsoft, Dell, HP and Lenovo entries use the maker's real vendor ID with a generic product ID, so for those only the name and maker string change. The generic entry uses a vendor ID reserved for open hardware.

## Something else

Choose Something else and fill in:

- **Name** (up to 24 characters, required): shown in the computer's Bluetooth list.
- **Maker** (up to 30 characters): the manufacturer string.
- **Model** (up to 30 characters): the model string.

Use printable characters only. A custom identity keeps the generic vendor and product IDs.

The default preset for a fresh board is set by `default_profile` in `jiggler.yaml` (0 to 9, the order above).

## What applying does

Applying an identity:

1. Stores the choice.
2. Removes the paired computer, because the old pairing belongs to the old identity.
3. Restarts the board (about 10 seconds).

Afterwards, remove the old entry from the computer's Bluetooth list and pair again.

## Good to know

- The name and IDs are labels. They do not make the device behave like that product, and they do not hide it from device-management software.
- Managed computers can log and restrict Bluetooth devices. Your employer's or school's rules apply to you.
