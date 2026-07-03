# Project 05 — CYD HaleHound Multi-Tool

## Status: ✅ COMPLETE — Daily Driver (replaced Marauder)

## Objective

Replace the ESP32 Marauder (retired — "boring, does 5 things") with a full-spectrum multi-tool firmware on the same ESP32 CYD hardware. HaleHound covers the same WiFi surface as Marauder **plus** Bluetooth offensive/defensive tools, SIGINT detection capabilities, and a gamified UI. It's a budget Flipper Zero on a $15 board.

## Why HaleHound Over Marauder

Marauder does the core WiFi handful (scan, deauth, evil portal, wardrive, sniff) and nothing else — flat utility, no depth. HaleHound is the same WiFi surface **plus** BLE attacks, SIGINT detection, and a gamified skull-themed UI with XP/levels and a "VALHALLA Protocol" panic wipe. Decision: HaleHound is the new primary CYD firmware. Marauder retired.

## Hardware

| Component | Details |
|-----------|---------|
| Board | ESP32-2432S028R (2.8" TFT, single micro-USB) |
| Firmware | HaleHound (latest stable) |
| GPS Module | NEO-7M (wiring: P1/GPIO3/9600 — pending verification) |
| Storage | MicroSD card |

## Firmware Flash Process

Tried five firmwares during the build process, learned the full Arduino/ESP32 toolchain along the way. HaleHound was the winner. Flashed and booted clean.

## Bare Board Capabilities (No Add-On Hardware)

### WiFi
- **Packet Monitor** — live 802.11 frame capture and analysis
- **Beacon Spammer** — broadcast fake SSIDs to demonstrate AP spoofing
- **WiFi Deauther** — force client disconnects (own network only)
- **Probe Sniffer** — capture device probe requests revealing network history
- **WiFi Scanner** — enumerate APs with SSID, BSSID, channel, signal, encryption
- **Captive Portal (GARMR)** — evil twin credential capture (replaces Marauder Evil Portal)
- **Station Scanner** — identify connected clients and their associated APs
- **Auth Flood** — authentication frame flood (lab/stress testing only)

### Bluetooth
- **BLE Spoofer** — impersonate BLE devices
- **BLE Beacon** — broadcast custom BLE beacons
- **BLE Predator** — GATT recon → clone → honeypot pipeline
- **WhisperPair** — exploits CVE-2025-36911 (BLE pairing vulnerability)
- **Airoha RACE** — exploits CVE-2025-20700 (Airoha chipset vulnerability)
- **Lunatic Fringe** — AirTag/tracker detection and analysis

### SIGINT / Detection
- **Flock You** — Flock Safety ALPR camera + Raven/ShotSpotter BLE beacon detection
- **IoT Recon** — LAN device scanner
- **Jam Detection** — RF jamming detection

## Known Limitations & Caveats

- **Flock You crash:** Currently crashes without GPS module connected. NEO-7M wiring (P1/GPIO3/9600) pending verification. Bug to be reported to HaleHound issues.
- **Flock detection caveat:** Flock You detects the *BLE beacon* that Flock cameras emit for fleet management. Not all roadside cameras are Flock — some are municipal/DOT traffic cameras with no BLE signature. A negative result doesn't mean no camera. Positive result confirms Flock ALPR specifically.
- **No Axon bodycam detection:** Despite the tool name suggesting broad surveillance detection, Axon bodycams use proprietary protocols not covered by the current BLE scan.

## Future "Full Send" Build (Money Permitting)

The full RF module stack for maximum capability. All planned, none purchased yet:
- **CC1101** — Sub-GHz transceiver (garage doors, weather stations, key fobs)
- **nRF24L01+** — 2.4 GHz raw RF (MouseJack keyboard/mouse attacks)
- **GPS NEO-7M** — geotagged scanning (wiring pending verification)
- **IR LED + Receiver** — infrared TX/RX
- **LoRa SX1278** — long-range mesh networking

## Defensive Takeaways

1. **BLE is a surveillance surface.** Devices constantly broadcast BLE beacons — AirTags, fitness trackers, headphones. Tools like Lunatic Fringe prove any nearby device can enumerate your BLE footprint.
2. **CVEs are weaponized fast.** WhisperPair (CVE-2025-36911) and Airoha RACE (CVE-2025-20700) went from disclosure to tool integration on a $15 board. Patch your firmware.
3. **ALPR cameras have BLE signatures.** Flock Safety cameras emit detectable BLE beacons for fleet management. This is a known OPSEC consideration for anyone concerned about surveillance infrastructure mapping.
4. **GARMR captive portals are more capable than Marauder's Evil Portal.** The credential capture page is built into the firmware with more customization. Defense remains the same: never trust captive portals, use VPN on public WiFi, verify SSIDs with staff.
5. **Panic wipe is real OPSEC.** VALHALLA Protocol demonstrates that offensive tools on seized hardware can be wiped. Blue teamers and forensic analysts should image devices before powering them on.

## OSCP Relevance

- Multi-tool proficiency maps to real engagement kit selection
- BLE attack surface is increasingly tested in modern pentests
- Understanding ALPR/surveillance detection feeds into physical security assessments
- The firmware evaluation process (testing 5 options, selecting based on capability matrix) mirrors real tool selection methodology

## Legal & Ethical Notes

- All active attacks (deauth, evil portal, auth flood, BLE exploits) performed exclusively against own devices on own network
- Passive scanning (WiFi enumeration, probe sniffing, BLE beacon detection, Flock detection) is legal — receiving publicly broadcast RF signals
- CVE tools (WhisperPair, Airoha RACE) tested only against own hardware
- Flock You is passive BLE detection only — no interaction with camera systems
