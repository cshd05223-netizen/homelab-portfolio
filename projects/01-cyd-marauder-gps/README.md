# Project 01 — CYD Marauder + GPS Wardriving Module

## Status: ✅ COMPLETE — Firmware Retired → [HaleHound](../05-cyd-halehound/)

> Marauder was the first firmware flashed on the CYD. It handled core WiFi tasks (scan, deauth, evil portal, packet monitor) and GPS wardriving with an external module. Retired once HaleHound replaced it with a broader feature set covering WiFi + BLE + SIGINT on the same board.

## Objective

Flash ESP32 Marauder firmware onto a Cheap Yellow Display (CYD) and integrate a GPS module for WiFi wardriving and geolocation-tagged network scanning.

## Hardware

| Component | Details |
|-----------|---------|
| Board | ESP32-2432S028R (CYD, 2.8", single micro-USB) |
| Firmware | ESP32 Marauder |
| GPS Module | BN-220 (wired: VCC→3.3V, GND→GND, TX→RX, RX→TX) |
| SD Card | FAT32 formatted, for wardriving logs |

## Build Process

1. Flashed Marauder via web flasher onto the CYD
2. Verified WiFi scanning, deauth, and packet monitor features on the touchscreen
3. Wired BN-220 GPS module to the CYD (4 wires: VCC, GND, TX→RX, RX→TX)
4. Confirmed GPS lock and wardriving output to SD card in WiGLE-compatible format
5. Tested scan + GPS logging while mobile

## Results

- WiFi scanning and deauth working on touchscreen interface
- GPS module achieved satellite lock and logged wardrive data to SD
- WiGLE-format CSV output confirmed on SD card
- Firmware retired when HaleHound offered the same WiFi features plus BLE and SIGINT

## Defensive Takeaways

- Wardriving shows how much network metadata leaks passively (SSID, BSSID, channel, signal strength, GPS coordinates)
- Hidden SSIDs don't prevent detection — the AP still responds to probes
- WPA3 and client isolation reduce the attack surface that tools like Marauder exploit
- Knowing what wardriving collects helps build detection rules for SIEM (unexpected probe responses, rogue APs)

## OSCP / Career Relevance

- Wireless reconnaissance methodology
- Understanding GPS-tagged network mapping
- Hardware integration (UART wiring, serial communication)
- Attack tool → defense awareness pipeline
