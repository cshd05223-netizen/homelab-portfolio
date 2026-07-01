# Project 01 — CYD Marauder + GPS Wardriving Module

## Status: ✅ COMPLETE (Firmware Retired — Replaced by [HaleHound](../05-halehound-cyd/))

> **Note:** Marauder has been retired as the primary CYD firmware. HaleHound now runs on Board 2, covering all of Marauder's WiFi capabilities plus BLE and SIGINT. This project documents the original Marauder build and remains as a reference.

## Objective

Build a portable WiFi reconnaissance and wardriving platform using an ESP32-2432S028R (Cheap Yellow Display) running ESP32 Marauder firmware with an integrated GPS module. The goal is to understand wireless network enumeration, probe request behavior, and geolocation mapping of access points — core skills in wireless penetration testing.

## Hardware

| Component | Details |
|-----------|---------|
| Board | ESP32-2432S028R (2.8" TFT, single micro-USB) |
| Firmware | ESP32 Marauder (flashed via web flasher) |
| GPS Module | Adafruit Ultimate GPS Breakout (PA1616D) |
| GPS Antenna | External active antenna (SMA connector) |
| Storage | MicroSD card for PCAP and GPS logs |
| Power | USB power bank for portable operation |

## Firmware Flash Process

1. Connected CYD via micro-USB
2. Used the ESP32 Marauder web flasher (Chrome/Edge, WebSerial API)
3. Selected the correct board variant (CYD — single micro-USB, 2.8" display)
4. Flashed successfully on first attempt

## GPS Integration

The GPS module was wired to the CYD's GPIO header to enable location-tagged scanning:

- GPS TX → CYD RX (GPIO pin)
- GPS RX → CYD TX (GPIO pin)
- VIN → 3.3V
- GND → GND

After wiring, GPS functionality was enabled through the Marauder menu. GPS lock was confirmed with satellite count displayed on the TFT screen.

## Capabilities

### WiFi Scanning
- **AP Scan:** Enumerates all access points in range with SSID, BSSID, channel, signal strength, and encryption type
- **Station Scan:** Identifies connected client devices and their associated APs
- **Probe Request Sniffing:** Captures probe requests from devices searching for known networks — reveals network history of nearby devices

### GPS-Enabled Wardriving
- Logs discovered access points with GPS coordinates
- Output compatible with mapping tools (Wigle, Google Earth KML export)
- Enables physical mapping of network infrastructure

### Attack Capabilities (lab use only)
- **Deauthentication:** Forces client disconnects from target AP (own network only)
- **Evil Portal:** Rogue captive portal for credential harvesting (own devices only)
- **Beacon Spam:** Broadcasts fake SSIDs to demonstrate AP spoofing

## Results

- Successfully scanned and enumerated all home network APs and clients
- GPS module locks and logs coordinates accurately
- Probe request captures revealed the extent of device network memory leakage
- Wardrive data collected for own neighborhood mapping exercise

## Defensive Takeaways

1. **Probe requests leak network history.** Devices constantly broadcast the names of networks they've previously connected to. Disable auto-join for networks you no longer use.
2. **Open networks are trivially spoofable.** Any SSID can be cloned. Never trust a network name alone — use VPNs on public WiFi.
3. **Deauthentication attacks are trivial.** WPA2 has no management frame protection. WPA3 and 802.11w (Protected Management Frames) mitigate this.
4. **Physical proximity matters.** All of these attacks require RF range. Physical security and RF shielding are real defensive controls.

## OSCP Relevance

- Wireless network enumeration is part of the OSCP+ wireless module
- Understanding probe requests and AP behavior feeds directly into social engineering and rogue AP attack chains
- The capture → analyze → attack pipeline mirrors real engagement methodology

## Legal & Ethical Notes

- All scanning performed on own network or in passive mode only
- Active attacks (deauth, Evil Portal) conducted exclusively against own devices with explicit authorization
- Passive scanning (AP enumeration, probe sniffing) is legal in the US as it involves only receiving publicly broadcast RF signals
