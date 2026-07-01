# Project 05 — HaleHound CYD Multi-Tool

## Status: ✅ FLASHED + RUNNING

## Objective

Replace ESP32 Marauder with HaleHound firmware on the primary CYD attack board — transforming it from a WiFi-only tool into a multi-protocol offensive platform covering WiFi, Bluetooth, and SIGINT capabilities. HaleHound is a budget-Flipper multi-tool with significantly more depth than Marauder's flat utility set.

## Why HaleHound Over Marauder

Marauder handles the core WiFi handful (scan, deauth, evil portal, wardrive, sniff) and nothing else. HaleHound covers that same WiFi surface **plus** Bluetooth and SIGINT modules, wrapped in a gamified skull-themed UI with XP/levels and a "VALHALLA Protocol" panic wipe. More attack surface to learn from, more defense scenarios to study.

**Decision: HaleHound is the new primary CYD firmware. Marauder retired.**

## Hardware

| Component | Details |
|-----------|---------|
| Board | ESP32-2432S028R (2.8" TFT, single micro-USB) |
| Firmware | HaleHound (replaces ESP32 Marauder) |
| Storage | MicroSD card for captures and logs |
| Planned Add-ons | GPS module, RF module (future "Full Send" build) |

## Capabilities — Bare Board (No Add-on Hardware)

### WiFi
| Tool | Function |
|------|----------|
| Packet Monitor | Live WiFi traffic monitoring |
| Beacon Spammer | Broadcast fake SSIDs |
| WiFi Deauther | Force client disconnects from target AP |
| Probe Sniffer | Capture device probe requests (reveals network history) |
| WiFi Scanner | Enumerate APs with SSID, BSSID, channel, signal, encryption |
| Captive Portal (GARMR) | Evil twin rogue AP with credential capture |
| Station Scanner | Identify connected clients and their associated APs |
| Auth Flood | Authentication frame flooding |

### Bluetooth
| Tool | Function |
|------|----------|
| BLE Spoofer | Spoof BLE device advertisements |
| BLE Beacon | Broadcast BLE beacon frames |
| BLE Predator | GATT recon → clone → honeypot attack chain |
| WhisperPair | Exploits CVE-2025-36911 |
| Airoha RACE | Exploits CVE-2025-20700 |
| Lunatic Fringe | AirTag / BLE tracker detection |

### SIGINT
| Tool | Function |
|------|----------|
| Flock You | Flock ALPR + Raven/ShotSpotter BLE detection |
| IoT Recon | LAN scanner for IoT device discovery |
| Jam Detection | RF jamming detection |

## Known Limitations

- **Flock detection caveat:** HaleHound's Flock You module is BLE-based. Per community research, Flock's BLE signature detection reportedly stopped working ~spring 2026 — current cameras are caught via WiFi wildcard-probe fingerprinting, not BLE. HaleHound will likely chase new methods, but it lags dedicated flock-you projects. Most reliable Flock detection today = deflock.me map + visual ID.
- **No Axon bodycam detection** — that was a PorkChop-only feature, confirmed absent from HaleHound source.
- **GPS and RF modules not yet installed** — bare board capabilities only for now. "Full Send" hardware expansion planned.

## Future Mod Project — "Full Send HaleHound"

Hardware expansion to unlock every HaleHound feature. Planned additions:
- GPS module for geolocated scanning
- RF module for expanded SIGINT capabilities
- Details and build log to be added when hardware is acquired

## Defensive Takeaways

1. **BLE is an attack surface most people ignore.** HaleHound's BLE Predator demonstrates that Bluetooth devices can be cloned and honeypotted. Disable BLE when not actively using it.
2. **CVE exploits on consumer hardware are real.** WhisperPair and Airoha RACE target real vulnerabilities in shipping Bluetooth stacks. Firmware updates matter.
3. **AirTags and BLE trackers can be detected.** Lunatic Fringe shows that tracker detection is feasible — and that trackers aren't invisible.
4. **IoT devices on your LAN are discoverable.** IoT Recon demonstrates why network segmentation matters — put IoT on a separate VLAN.
5. **Multi-protocol attacks compound risk.** A single device covering WiFi + BLE + SIGINT shows how attackers chain different protocols. Defense-in-depth across all wireless protocols, not just WiFi.

## OSCP Relevance

- Multi-protocol reconnaissance methodology (WiFi + BLE + SIGINT)
- Understanding CVE-based exploitation on embedded devices
- Evil twin / rogue AP attack chains (GARMR captive portal)
- Tool proficiency with offensive hardware platforms
- Real-world attack surface assessment skills

## Legal & Ethical Notes

- All active attacks (deauth, evil twin, BLE attacks) performed exclusively on own devices and own network
- Passive scanning and monitoring is legal in the US (receiving publicly broadcast RF signals)
- CVE testing (WhisperPair, Airoha RACE) only against own Bluetooth devices
- BLE Predator honeypots only deployed in own lab environment
