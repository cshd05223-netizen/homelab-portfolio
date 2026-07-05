# Cybersecurity Homelab Portfolio

A hands-on security lab built on physical hardware. Blue-team defense is the career path; red-team offense is the hobby that feeds it. Every project documents methodology, results, and defensive takeaways — no fabricated data.

**Current Phase:** Device Setup — hardware arriving, flashing, and integrating before the first full attack-and-detect exercise.

## About Me

21-year-old cybersecurity student at Lackawanna College pursuing a career as a Security Engineer via the SOC analyst path. I learn by building — every device in this lab exists to teach me how attacks work so I can detect and stop them.

**Cert track:** Security+ → CCNA → CySA+ (OSCP long-term goal)
**Study:** HTB Information Security Foundations → SOC Analyst path

## Lab Environment

| Role | Hardware | OS / Firmware | Purpose |
| --- | --- | --- | --- |
| Main Rig | AMD Ryzen 7 5700X, RX 6650 XT (8GB), 16GB RAM | Pop!_OS 24.04 | Analysis, SIEM VM host, hash cracking |
| SIEM Box | Used office PC (sourcing) | Security Onion / Wazuh (planned) | Dedicated SIEM — log aggregation, detection |
| Recon Laptop | HP Pavilion i3 | Parrot OS (planned dual-boot) | Portable recon / capture |
| Target Box | HP Stream Laptop | Windows | Authorized victim for generating telemetry |
| Red Attack Board | ESP32 CYD (2432S028R) | HaleHound v3.6.1 | WiFi/BLE/SIGINT attacks |
| Passive Hunter | ESP32 CYD (2432S028R) | FancyGotchi | Passive WPA/PMKID farming |
| Blue Sensor Board | ESP32 CYD (dead touch, headless) | Custom sketch (planned) | Honeypot / deauth-detector / SIEM feed |
| RF Multi-Tool | Flipper Zero | Stock (custom TBD) | Sub-GHz, NFC/RFID, IR, BLE breadth |
| Flock Detector | M5 Atom Lite (inbound) | flock-you | ALPR / surveillance detection |
| RF Expansion | HaleHound Cinder Ferret V2 (inbound) | — | CC1101 + NRF24 + GPS add-on for HaleHound CYD |
| T-Embed | LILYGO T-Embed CC1101 (inbound) | Bruce (planned) | Sub-GHz / WiFi / BLE |

## Projects

### Blue Team (Career Track)

| # | Project | Status |
|---|---------|--------|
| 06 | [SIEM Homelab (Flagship)](projects/06-siem-homelab/README.md) | 📋 Planned — sourcing hardware |
| 09 | [Blue Sensor CYD](projects/09-blue-sensor-cyd/README.md) | 📋 Planned — dead-touch board allocated |

### Red Team (Offense → Defense Learning)

| # | Project | Status |
|---|---------|--------|
| 01 | [CYD Marauder + GPS](projects/01-cyd-marauder-gps/README.md) | ✅ Complete (firmware retired → HaleHound) |
| 02 | [CYD FancyGotchi](projects/02-cyd-fancygotchi/README.md) | ✅ Complete + running (board → future blue sensor) |
| 05 | [CYD HaleHound Multi-Tool](projects/05-cyd-halehound/README.md) | ✅ Flashed + working |
| 07 | [Atom Lite — Flock Detector](projects/07-atom-lite-flock/README.md) | 📋 Placeholder — device inbound |
| 08 | [Flipper Zero](projects/08-flipper-zero/README.md) | 📋 Placeholder — device owned |

### Backlog (not started — get folders when work begins)

- **Evil Portal** — Captive portal attack simulation using HaleHound GARMR. Own phone / own network only.
- **WPA2 Hashcat Crack** — Full capture-to-crack pipeline using FancyGotchi .pcap + Hashcat on RX 6650 XT.

## Methodology

Every completed project follows this structure:
1. **Objective** — what and why
2. **Hardware / Software** — bill of materials
3. **Build Process** — step-by-step with troubleshooting
4. **Results** — what actually happened (no fabrication)
5. **Defensive Takeaways** — how to detect / prevent this attack
6. **OSCP / Career Relevance** — how it maps to certs and job skills

## Legal & Ethics

All testing is performed on hardware and networks I own. Captures that include neighbor traffic are identified and excluded from any cracking or analysis. This lab exists to learn offense so I can build better defense.

## Contact

- GitHub: [@cshd05223-netizen](https://github.com/cshd05223-netizen)
