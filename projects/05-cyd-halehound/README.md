# Project 05 — CYD HaleHound Multi-Tool

## Status: ✅ FLASHED + WORKING — Cinder Ferret V2 mod pending

## Objective

Replace the retired ESP32 Marauder with HaleHound — a full-spectrum WiFi/BLE/SIGINT multi-tool on the same CYD hardware. This board becomes the primary red-team attack platform feeding telemetry into the [SIEM](../06-siem-homelab/) for blue-team detection work.

## Hardware

| Component | Details |
|-----------|---------|
| Board | ESP32-2432S028R (CYD, 2.8", CH340 USB) — formerly "Board 2" (was Marauder) |
| Firmware | HaleHound-CYD v3.6.1 (JesseCHale) — ESP32-DIV HaleHound Edition |
| Pending Mod | HaleHound Cinder Ferret V2 — CC1101 + NRF24 + GPS add-on (inbound, ~$40) |

## Build Process

1. Connected CYD via USB (CH340 serial)
2. Flashed HaleHound-CYD v3.6.1 via web flasher
3. Confirmed boot and touchscreen working (this is the good-touch board)
4. Verified bare-board WiFi and BLE features

## Bare Board Capabilities (working now)

**WiFi:**
- AP scanning, station scanning, deauth (targeted + broadcast)
- GARMR captive portal (evil portal — feeds Evil Portal backlog project)
- Beacon spam, probe request sniffing, packet monitor
- EAPOL/PMKID handshake capture → saved to /eapol/ on SD

**BLE:**
- BLE scanning, device enumeration
- Skimmer detection (gas pump / ATM BLE skimmer alerts)
- BLE spam (notification flooding for testing)
- AirTag scanner

**SIGINT:**
- Pager interception (POCSAG/FLEX decoding)
- Signal detection and analysis

## Features Requiring Cinder Ferret V2 Add-On (TODO)

These unlock when the Cinder Ferret V2 module arrives and is wired in:

- **NRF24:** BLE Cinder jammer, NRF Sniffer, MouseJack, WLAN Ember, Proto Kill
- **CC1101 / Sub-GHz:** .sub replay, SubGHz Scorch, brute force, Tesla charge port, spectrum
- **PN532:** NFC/RFID scan, clone, emulate, MIFARE
- **GPS:** Wardriving with WiGLE output, GPS-tagged Flock detections, GPS live view

### Cinder Ferret V2 Install Notes (placeholder)

- TODO: Document ribbon/JST cable wiring when module arrives
- TODO: Verify power requirements (USB-C PD or external 5V?)
- TODO: Test each feature category post-install

## Hardware Lane Split

| Job | Tool |
|-----|------|
| WiFi recon, deauth, handshakes, captive portal | CYD + HaleHound (this board) |
| Passive WPA/PMKID farming | CYD + FancyGotchi (Board 1) |
| Sub-GHz, NFC, RFID, IR, BLE breadth | Flipper Zero |
| Hash cracking | Main rig (Ryzen + RX 6650 XT) |

## Known Caveats

- **Flock You crash:** The Flock detection feature may cause the CYD to restart due to memory constraints. The M5 Atom Lite (separate device, inbound) is the dedicated Flock detector — don't rely on the CYD for this.
- **No Axon bodycam detection:** HaleHound does not detect Axon body cameras despite some community claims. Not a real feature.

## Defensive Takeaways

- Deauth attacks exploit the unprotected nature of 802.11 management frames — 802.11w (Protected Management Frames) is the defense
- GARMR captive portals demonstrate why users should never enter credentials on public WiFi login pages
- BLE skimmer detection shows that attackers use commodity Bluetooth to exfiltrate card data from gas pumps and ATMs
- Pager interception (POCSAG/FLEX) reveals that pager traffic is unencrypted broadcast — hospitals and emergency services still use these

## Legal & Ethics

All testing on owned hardware and networks only. Deauth, evil portal, and capture features are used exclusively against my own devices. The lane split above ensures each tool has a clear, scoped role.

## OSCP / Career Relevance

- Multi-vector wireless attack methodology (WiFi + BLE + SIGINT on one device)
- Understanding the relationship between attack capabilities and detection opportunities
- Hardware modding and firmware management
- Building attack telemetry that feeds blue-team detection (SIEM integration)
