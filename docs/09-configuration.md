---
title: Configuration
order: 9
summary: Every substitution and option in jiggler.yaml.
---

# Configuration

Everything the page cannot change lives in `jiggler.yaml`. Edit it, then rebuild and flash (or upload `firmware.ota.bin` from the page).

## Substitutions

The `substitutions:` block at the top of the file.

| Name | Default | Meaning |
|---|---|---|
| `devicename` | `mouse-jiggler` | Base name. The real name adds the last six characters of the MAC address, for example `mouse-jiggler-a1b2c3`. It becomes the setup network name and the `.local` address. |
| `friendly_name` | `Mouse Jiggler` | Display name. |
| `default_profile` | `0` | Mouse preset used before anything is chosen on the page (0 to 9). |
| `default_min_interval` | `120` | Shortest wait in seconds before anything is stored. |
| `default_max_interval` | `180` | Longest wait in seconds. |
| `default_amplitude` | `2` | Nudge size in pixels (1 to 5). |
| `pairing_timeout` | `120s` | How long pairing mode stays open. |
| `setup_password` | empty | Password for the setup network. Empty means open. If set, 8 characters or more. |
| `firmware_version` | `1.0.0` | Version shown in the page footer. |
| `pin_sda`, `pin_scl` | `GPIO5`, `GPIO6` | OLED I2C pins. |
| `pin_led`, `led_inverted` | `GPIO8`, `true` | Status LED pin and polarity. |
| `pin_button` | `GPIO9` | BOOT button pin. |
| `i2c_address` | `0x3C` | OLED address. |
| `oled_model` | `SSD1306 72x40` | Display model. |
| `oled_offset_x`, `oled_offset_y` | `0`, `0` | Shift the image if it is clipped. |
| `oled_flip_x`, `oled_flip_y` | `true`, `true` | Flip if the image is mirrored or upside down. |
| `oled_test_pattern` | `false` | Show a border, a diagonal cross and corner blocks. |

The defaults for profile, wait range and size only apply on a board that has never stored settings. After that, the page's values win. To apply new defaults to a board, use Erase all settings on the page.

## The `jiggler_portal` options

```yaml
jiggler_portal:
  id: portal
  mouse_id: mouse               # required: the ble_hid_mouse to control
  default_profile: 0
  default_min_interval: 120
  default_max_interval: 180
  default_amplitude: 2
  firmware_version: "1.0.0"
  weak_signal_recovery: true    # see Wi-Fi behavior
```

The configuration check requires `wifi: ap:` to be present and rejects a fixed `ssid` or `networks:` list in `wifi:`, because the page stores networks on the device.

## The `ble_hid_mouse` options

`pairing_timeout`, and the identity options `manufacturer`, `model`, `vendor_id_source`, `vendor_id`, `product_id`, `product_version` and `battery_level`. In this edition the identity is chosen at runtime by the portal, so these are overridden at boot by the portal's stored choice. The Bluetooth name comes from `esp32_ble: name`, and is likewise replaced.

## Using local components

By default `jiggler.yaml` fetches the two components from GitHub:

```yaml
external_components:
  - source: github://witoldwitkowski/esphome-ble-jiggler-standalone@main
    components: [ble_hid_mouse, jiggler_portal]
```

To build from a checkout instead, replace it with:

```yaml
external_components:
  - source:
      type: local
      path: components
    components: [ble_hid_mouse, jiggler_portal]
```

Pin a release by replacing `@main` with a tag once one exists.

## Other ESPHome options worth knowing

| Setting | Why |
|---|---|
| `wifi: power_save_mode: light` | Leaves airtime for Bluetooth. Do not set it to `none`. |
| `wifi: fast_connect: false` | Always scan, so the best saved network wins. |
| `wifi: post_connect_roaming: true` | Enables built-in roaming. |
| `wifi: reboot_timeout: 0s` | Never restart because the router is away. |
| `preferences: flash_write_interval: 5min` | Limits flash writes from other components. The portal writes its own settings straight away. |
