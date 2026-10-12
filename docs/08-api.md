---
title: HTTP API
order: 8
summary: The endpoints behind the web page, for scripts and home-grown dashboards.
---

# HTTP API

The page is a client of a small HTTP API. You can call it from scripts too. There is no authentication; see [Security](./11-security.md).

Replace `HOST` with `mouse-jiggler-xxxxxx.local` or the board's IP address.

## Reading

### `GET /api/state`

A JSON snapshot, rebuilt once per second.

```json
{
  "fw": "1.0.0", "esphome": "2026.9.1", "name": "mouse-jiggler-a1b2c3", "tag": "a1b2c3",
  "up": 86400, "heap": 142000, "on": true,
  "bt":  {"link": true, "ready": true, "bond": true, "pair": false, "left": 0, "idle": 0},
  "j":   {"next": 102, "prog": 0.43, "count": 7, "ago": 14, "min": 120, "max": 180, "amp": 2},
  "w":   {"up": true, "ap": false, "saved": 1, "ip": "192.168.1.23", "host": "mouse-jiggler-a1b2c3.local",
          "ssid": "Maple Street", "rssi": -58, "bars": 3, "ch": 6, "roam": false},
  "nets": [{"ssid": "Maple Street", "hidden": false}],
  "id":  {"profile": 0, "name": "MX Anywhere 3", "cname": "", "cmfr": "", "cmodel": ""},
  "cfg": {"dim": 60, "days": 0, "display": true}
}
```

| Field | Meaning |
|---|---|
| `up` | Uptime in seconds. |
| `on` | Keep awake switch. |
| `bt.ready` | Connected, encrypted and subscribed: a nudge can be delivered. |
| `bt.link` | Raw link state. |
| `bt.bond` | A computer is paired. |
| `bt.pair`, `bt.left` | Pairing mode is open, and seconds left. |
| `bt.idle` | Seconds since the computer disconnected. |
| `j.next` | Seconds until the next nudge, or -1 when none is scheduled. |
| `j.ago` | Seconds since the last nudge, or -1 if none yet. |
| `w.up` | Connected to a router. |
| `w.ap` | The setup network is up. |
| `w.trying` | Present while not connected: seconds spent trying. |
| `w.roam` | A roaming scan is in progress. |
| `id.profile` | Preset index 0 to 9, or -1 for a custom identity. |

### `GET /api/scan`

The latest Wi-Fi scan: `{"aps": [{"ssid": "...", "rssi": -52, "bars": 4, "lock": true, "saved": false, "ch": 6}]}`, strongest first, duplicates merged.

### `GET /api/profiles`

The ten presets as `[{"i":0,"label":"Logitech MX Anywhere 3","name":"MX Anywhere 3","mfr":"Logitech","ids":"046D:B025"}, ...]`.

## Writing

All writes are `POST` with a form-encoded body (`application/x-www-form-urlencoded`). Success returns `{"ok":true}`. Invalid input returns HTTP 400 with `{"ok":false,"error":"..."}`.

| Endpoint | Parameters | Effect |
|---|---|---|
| `/api/enable` | `on=1` or `0` | Keep awake on or off. |
| `/api/timing` | `min`, `max` (10 to 3600 s), `amp` (1 to 5) | Wait range and size. A larger `min` raises `max`. |
| `/api/jiggle` | none | Send one nudge now. |
| `/api/pair` | none | Open pairing mode. |
| `/api/pair_stop` | none | Close pairing mode. |
| `/api/forget` | none | Forget the computer and open pairing mode. |
| `/api/profile` | `id` = 0 to 9 or `custom`; for custom: `name`, `mfr`, `model` | Change identity, forget the computer, restart. |
| `/api/scan_start` | none | Start a scan (only when nothing is saved and not connected). |
| `/api/wifi_add` | `ssid`, `psk`, `hidden=1` (optional) | Save and try a network. Password 8 to 63 characters, or empty for an open network. |
| `/api/wifi_remove` | `ssid` | Remove a saved network and restart. |
| `/api/wifi_reset` | none | Forget all networks and restart into setup mode. |
| `/api/device` | `dim` (0 to 3600 s), `days` (0 to 365), `display=0` or `1` | Screen and automatic-restart settings. |
| `/api/restart` | none | Restart. |
| `/api/factory` | none | Erase settings, networks and pairing, then restart. |

Examples:

```
curl -d on=1 http://HOST/api/enable
curl -d min=60 -d max=90 -d amp=2 http://HOST/api/timing
curl http://HOST/api/state
```

Mutations are applied on the board's main loop shortly after the reply, so read `/api/state` again to see the result.

## Firmware update

`POST /update` with a multipart upload of `firmware.ota.bin` (the page's Update firmware link opens the form).
