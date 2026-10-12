---
title: Hardware
order: 10
summary: The supported board, pins, and the display bring-up test.
---

# Hardware

## The board

An ESP32-C3 development board with a 0.42-inch OLED (72 x 40 visible pixels, SSD1306-compatible), a BOOT button and a user LED, with USB-C. The configuration uses the `esp32-c3-devkitm-1` board definition with 4 MB flash.

## Default pins

| Function | Pin | Notes |
|---|---|---|
| OLED SDA | GPIO5 | |
| OLED SCL | GPIO6 | |
| LED | GPIO8 | A strapping pin; used as an output after boot. Inverted. |
| BOOT button | GPIO9 | A strapping pin; only read after boot. Active low with pull-up. |
| OLED I2C address | 0x3C | |

These are common defaults for this kind of board and are **not confirmed for any specific board**. Check yours with the I2C scan (below) and change the substitutions in `jiggler.yaml`.

## Bring-up

1. Flash and open the logs (`esphome logs jiggler.yaml`). With `i2c: scan: true` you should see `Found i2c device at address 0x3C`. If nothing is found, the pins are wrong for your board.
2. Set `oled_test_pattern: "true"` and flash. The display shows a border, a diagonal cross, and a solid block in each corner of the 72 x 40 window. All four blocks must be visible.
3. If the image is shifted or cut off, adjust `oled_offset_x` and `oled_offset_y`, or try `SH1106 128x64` as `oled_model`.
4. If it is upside down or mirrored, toggle `oled_flip_x` and `oled_flip_y`.
5. Set `oled_test_pattern` back to `"false"`.

## Power

A wall USB adapter is the most reliable. If the board is powered from a computer's USB port, power may be reduced or cut when that computer sleeps, and the jiggler stops with it.

## Resource use

Measured with ESPHome 2026.9.1: flash 69.9% of one OTA slot, RAM 42.6%. Two OTA slots of 1.8 MB each plus space for settings and Bluetooth bonds fit in 4 MB.

## Radio

2.4 GHz Wi-Fi and Bluetooth LE share one radio. Light Wi-Fi power save is on to keep both stable. If the Bluetooth link drops often, move the board closer to the router, or away from a busy 2.4 GHz channel.
