---
title: Overview
order: 1
summary: What the jiggler is, who it is for, and what you get.
---

# Overview

The Mouse Jiggler is a small ESP32-C3 board that pairs with a computer over Bluetooth LE and behaves like a Bluetooth mouse. When you switch it on, it nudges the cursor a pixel or two and straight back, at a random interval, so the computer sees activity and does not go idle.

The standalone edition needs nothing else: no Home Assistant, no router changes, no apps. You set it up from your phone, and from then on you control it from a web page served by the board itself.

## Features

**Setup from a phone**
- On first boot the board creates its own Wi-Fi network. Join it and a setup page opens.
- The page lists the Wi-Fi networks the board can hear. Tap one, type the password, done.
- The board remembers up to five networks and joins the strongest one in range.

**Control from the web page**
- A master Keep awake switch.
- Pair with a computer, or forget the paired computer.
- Timing: the shortest and longest wait between nudges (10 seconds to 1 hour) and the nudge size (1 to 5 pixels).
- A Nudge now button for testing.
- A live copy of the board's own OLED screen at the top of the page.

**Choose which mouse it pretends to be**
- Ten presets: five Logitech models, Microsoft, Dell, HP, Lenovo, and a generic mouse.
- Or type your own name and maker.

**Wi-Fi that copes with real homes**
- Roams between access points of the same network (mesh systems, several routers sharing a name).
- Recovers by itself if it gets stuck on a weak signal.
- Brings the setup network back if every saved network disappears, for example after you move house or change router.

**Works without Wi-Fi**
- Pairing, jiggling and the button work with no Wi-Fi at all. Wi-Fi only gives you the page.

**Maintenance**
- Firmware updates from the browser.
- Optional automatic restart every 1, 3 or 7 days.
- Every setting survives power cuts.

## What it does not do

- It is a mouse only. It never presses a button, scrolls, or types a key.
- It does not connect to more than one computer. A new pairing replaces the old one.
- It has no cloud service, account, or phone app.
- It does not support Wi-Fi networks that need a username (WPA2-Enterprise).

## Hardware

An ESP32-C3 board with a built-in 0.42-inch (72 x 40 pixel) OLED, a BOOT button and an LED, for example the "ESP32-C3 0.42 OLED" boards sold under several brand names. See [Hardware](./10-hardware.md) for the pin assumptions.

## Where to go next

- [Getting started](./02-getting-started.md): flash the board and set it up.
- [Using the web page](./03-web-page.md): every control explained.
- [How it works](./05-how-it-works.md): the design, for the curious.
