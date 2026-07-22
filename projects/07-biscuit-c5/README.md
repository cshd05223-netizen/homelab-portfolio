# 07 — Biscuit Wardriving Node (XIAO ESP32-C5) — Complete

## Objective

Build the cheapest viable wardriving + surveillance-detection node — a single dual-band ESP32 board controlled entirely from my phone — instead of buying an expensive commercial puck. Consolidate wardriving/Flock-detection onto one strong, phone-controlled device.

## Hardware / Software

- **Board:** XIAO ESP32-C5 (~$8) — dual-band 2.4GHz / 5GHz WiFi, 8MB flash + 8MB PSRAM
- **Firmware:** DIY Biscuit (merges the dual-chip Biscuit Pro design onto a single ESP32-C5)
- **Control:** free Biscuit app on the phone, over Bluetooth
- **GPS:** sourced from the phone over Bluetooth — no onboard GPS module needed
- **Flashing:** `flasher.biscuitshop.us` / ESP Terminator over Chrome (WebSerial), board target "Biscuit DIY Dev (ESP32-C5)"

## Why a Single C5 (design reasoning)

- The commercial Biscuit Pro pairs a C5 with a second ESP32-WROOM only so the C5 never has to pause scanning to talk to the phone — a performance optimization, not a requirement.
- The DIY firmware runs everything on one C5. You lose a little throughput during heavy operations but keep full functionality.
- The C5's dual-band (2.4/5GHz) radio covers more of the modern spectrum than a stack of older single-band ESP32s — better for wardriving specifically, with far less hardware.
- No WROOM, no GPS module, no male-male USB-C link needed. The phone provides GPS and control over Bluetooth.

## Build Process

1. Bought a single XIAO ESP32-C5 (8MB PSRAM — the spec that actually matters; a bare C5 without PSRAM won't run the firmware).
2. Flashed DIY Biscuit firmware via the web flasher over Chrome (same WebSerial workflow as previous CYD/Flipper flashes; user already in the `dialout` group).
3. Paired the board to the Biscuit phone app over Bluetooth.
4. Confirmed the board comes up in the app and scans.

## Results

- Flashed clean and the board shows up and runs in the Biscuit phone app.
- Wardriving works, controlled entirely from the phone with phone-sourced GPS — no puck, no extra modules.
- **Kit consolidation:** this single phone-controlled device covers the wardriving + Flock/surveillance-detection role that previously would have leaned on the HaleHound CYD and other boards. Cheaper, simpler, and controllable from my pocket.

## Kit Division (after consolidation)

| Device | Role |
|--------|------|
| Biscuit C5 | Wardriving + Flock/surveillance detection (phone-controlled, dual-band) |
| M5 Atom Lite (eye-spy) | Dedicated passive counter-surveillance detector |
| Flipper Zero | Sub-GHz, NFC, IR, BadUSB, iButton, light BLE |
| HaleHound CYD | WiFi/BLE learning + experimentation (wardriving role superseded by the C5) |

## Defensive Takeaways

- Wardriving shows how much a network reveals just by broadcasting (SSID, signal, location) — the defensive lesson is to minimize broadcast exposure and understand that AP mapping is trivial for anyone nearby.
- Dual-band scanning matters: detection/monitoring tools blind to 5GHz miss a growing share of real traffic.
- Kit discipline: one strong, well-chosen device beat a stack of overlapping boards. Choosing the right tool over more tools mirrors real security-team budget discipline.
