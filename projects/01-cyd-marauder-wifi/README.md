# CYD Marauder WiFi Toolkit Build

## Objective

Build a portable WiFi reconnaissance platform using ESP32 Cheap Yellow Display (CYD) hardware running Marauder firmware. Understand wireless network enumeration, probe request sniffing, and beacon analysis from the attacker's perspective.

## Hardware

| Component | Model | Cost | Notes |
|-----------|-------|------|-------|
| Microcontroller | ESP32-2432S028R (Cheap Yellow Display) x2 | ~$15 ea | 2.8" TFT touchscreen, built-in WiFi, SD card slot |
| Antenna | External 2.4GHz antenna (soldered) | — | Extended range over PCB antenna |
| Storage | MicroSD cards | — | Log storage for captures |
| Power | USB-C power bank | — | Field-portable power |

## Build Process

### Step 1: Firmware Flash

Flashed ESP32 Marauder firmware onto both CYDs using the web flasher at [marauder.riskybirds.com](https://marauder.riskybirds.com).

**Key decisions:**
- Selected the CYD-specific build (not generic ESP32) for touchscreen support
- Used Chrome/Edge for Web Serial API compatibility
- Held BOOT button during flash initiation for download mode

### Step 2: SD Card Setup

Formatted MicroSD as FAT32 and inserted into the CYD. Marauder uses the SD card for:
- Saving scan results (WiFi networks, probe requests)
- Storing Evil Portal HTML files
- GPS coordinate logging (with GPS module attached)
- PCAP packet captures

### Step 3: Initial Scanning

Powered on and ran WiFi scans against my own home network to verify functionality:
- **Scan APs** — Enumerated all visible access points (SSID, BSSID, channel, signal strength, encryption type)
- **Scan Stations** — Identified connected client devices
- **Probe Request Sniffing** — Captured probe requests showing SSIDs devices are actively searching for

## Results

Both CYDs successfully flashed and operational. WiFi scanning confirmed detection of:
- Home network AP with WPA2 encryption
- Neighboring networks (for passive enumeration only)
- Client device probe requests revealing previously connected network names

## Defensive Takeaways

1. **Probe requests leak information** — Devices broadcast the names of networks they've previously connected to. Anyone with a $15 CYD can see what WiFi networks your phone is looking for. **Mitigation:** Regularly clear saved networks, disable auto-connect.

2. **Hidden SSIDs aren't hidden** — A "hidden" network still responds to directed probe requests from clients. Marauder reveals these trivially. **Mitigation:** Don't rely on SSID hiding as a security measure.

3. **Open networks are trivially spoofable** — If your device probes for "Starbucks_WiFi", an attacker can stand up a rogue AP with that exact name and your device will auto-connect. This is the foundation of the Evil Portal attack (Project 03).

4. **Channel and signal intel matters** — Knowing what channel an AP operates on and its signal strength helps an attacker position for deauth or capture attacks. Defenders should monitor for rogue APs on their channels.

## OSCP Relevance

- Wireless reconnaissance methodology
- Understanding of 802.11 frame types (beacons, probes, authentication)
- Attack surface enumeration — the first phase of any engagement
- Demonstrates hands-on hardware skills beyond software-only pentesting

## Tools Used

- ESP32 Marauder firmware
- ESP32-2432S028R (CYD) hardware
- Web Serial API flasher
