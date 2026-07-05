# Project 08 — Flipper Zero

## Status: 📋 PLACEHOLDER — Device Owned (arriving July, birthday)

## Objective

Integrate the Flipper Zero into the homelab as the dedicated RF multi-tool covering frequency bands and protocols that the CYD boards physically cannot handle: Sub-GHz, NFC/RFID, infrared, and BLE.

## Hardware

| Component | Details | Status |
|-----------|---------|--------|
| Device | Flipper Zero | Incoming (birthday, July) |
| Firmware | Stock (custom TBD) | Decision pending |

## Hardware Lane Split

The Flipper Zero fills gaps that the CYD-based tools don't cover:

| Job | Tool |
|-----|------|
| WiFi recon, deauth, handshakes | CYD + HaleHound |
| Passive WPA/PMKID farming | CYD + FancyGotchi |
| **Sub-GHz, NFC, RFID, IR, BLE breadth** | **Flipper Zero (this device)** |
| Hash cracking | Main rig |

## Planned Capabilities

- **Sub-GHz:** Signal capture, replay, analysis (garage doors, remotes, keyfobs — own devices only)
- **NFC/RFID:** Card reading, emulation, UID cloning (own cards only)
- **Infrared:** Universal remote, signal learning, device control
- **BLE:** Scanning, enumeration, advertisement analysis
- **GPIO:** Hardware interface for additional modules

## Build Process

TODO — document when device arrives:
1. Initial setup and firmware decision (stock vs custom)
2. Sub-GHz testing against own devices
3. NFC/RFID testing with own cards
4. Integration with lab workflow

## Results

TODO — no fabricated data.

## Defensive Takeaways

TODO — will cover: why rolling codes matter, NFC/RFID cloning risks, physical security implications.

## Legal & Ethics

All Sub-GHz, NFC, and RFID testing on personally owned devices and cards only. No unauthorized signal replay or card cloning.

## OSCP / Career Relevance

- Physical security assessment methodology
- RF protocol analysis
- Understanding multi-vector attack surfaces beyond WiFi
