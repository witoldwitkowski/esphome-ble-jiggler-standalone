---
title: Button, LED and screen
order: 7
summary: BOOT button gestures, the LED, and what each OLED screen means.
---

# Button, LED and screen

## The BOOT button

| Press | What happens |
|---|---|
| Tap | Wakes the screen and shows the network address for 8 seconds. |
| Hold 3 seconds | Opens pairing mode. |
| Hold 10 seconds | Forgets the paired computer and opens pairing mode. |
| Hold 20 seconds | Forgets all Wi-Fi networks and restarts into setup mode. |

Release the button within the range for the action you want: 3 to 9 seconds pairs, 10 to 19 seconds forgets the computer, 20 seconds or more resets Wi-Fi.

The Wi-Fi reset is your way back in if you ever lock yourself out, for example after moving house.

## The LED

- Slow blink: the setup network is up.
- Dark: otherwise.

## The OLED

The normal screen has three icons across the top and a progress line along the bottom.

| What you see | Meaning |
|---|---|
| Bluetooth symbol, solid block | A computer is paired and connected. |
| Bluetooth symbol, slashed and blinking slowly | Paired, but the computer is not connected right now (asleep or out of range). |
| Bluetooth symbol with waves growing on both sides | Pairing mode is open. |
| Bluetooth symbol with a small plus | Nothing is paired. |
| Mouse icon, filled or empty | Keep awake is on or off. It pulses briefly when a nudge is sent. |
| Wi-Fi bars | Signal strength to your router. Empty bars when not connected. |
| Progress line along the bottom | How far through the wait before the next nudge. Only shown while on and connected to a computer. |

Full-screen displays take over the normal screen, in this order of priority:

| Screen | When |
|---|---|
| Firmware update bar | A firmware update is in progress. |
| Broadcast arcs and six characters | The setup network is up. The characters are the end of its name. |
| IP address in large digits, wrapped at a dot | For 20 seconds after connecting to Wi-Fi, and for 8 seconds after tapping BOOT. |

The screen can be turned off or dimmed from the page. There is no Home Assistant icon in this edition.

The screens are drawn from shapes and a tiny built-in digit font, with no image files. They have not been seen on a real panel yet; see [Status](./13-status.md).
