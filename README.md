# Cybersecurity Homelab Portfolio

A hands-on security lab built on physical hardware I own. **Blue-team defense is the career path; red-team offense is the hobby that feeds it.** Every project documents methodology, results, and defensive takeaways — no fabricated data.

> **Current phase:** Building and integrating the detection lab. The SIEM lab core is up; the full attack-and-detect automation layer is in progress.

## About Me

Cybersecurity student on a **blue-team career track — SOC Analyst -> Security Engineer.** Night-shift hospital supply chain worker building practical defensive security skills through hands-on lab work. I attack my own isolated lab specifically so I can learn to *detect and defend* against those attacks (a purple-team method in service of a blue-team career).

- **Career target:** SOC Analyst (entry) -> Security Engineer
- **In progress:** CompTIA Security+ (primary cert), CCNA
- **Training platforms:** HackTheBox (SOC Analyst path), TryHackMe
- **Focus areas:** SIEM & log analysis, detection engineering, network security monitoring, wireless/RF security

## Lab Environment

| Role | Hardware | OS / Firmware | Purpose |
| --- | --- | --- | --- |
| Main Rig | AMD Ryzen 7 5700X, RX 6650 XT (8GB), **16GB RAM** | Pop!_OS 24.04 | SIEM host, hypervisor, VM lab |
| Attacker VM | (virtual) | Kali 2026.2 | Attacks against the isolated lab |
| Target VM | (virtual) | Metasploitable 2 | Vulnerable victim for attack/detect |
| SIEM | (Docker on host) | Wazuh 4.9.0 | Log analysis, detection, dashboards |
| Counter-surveillance | M5 Atom Lite (ESP32-PICO-D4) | eye-spy | Passive BLE/WiFi surveillance detector |
| Multi-protocol RF | ESP32 CYD (2.8") | HaleHound | WiFi/BLE/Sub-GHz security auditing |
| RF/NFC/Sub-GHz | Flipper Zero | *(incoming)* | Sub-GHz, NFC, RFID, IR |

## Projects

### Current

- **[01 - SIEM Homelab (Wazuh)](projects/01-siem-homelab)** *(flagship, in progress)* - Isolated detection lab: Wazuh SIEM, sealed virtual network, Kali attacker, Metasploitable target. Attack -> detect -> defend.
- **[02 - Counter-Surveillance Detector (M5 Atom Lite / eye-spy)](projects/02-m5-eyespy-counter-surveillance)** *(complete)* - Passive BLE + WiFi detector for surveillance/tracking devices (ALPR cameras, AirTags, body cams). Defensive privacy tool.
- **[03 - HaleHound CYD](projects/03-halehound-cyd)** *(complete)* - ESP32 Cheap Yellow Display flashed with HaleHound for multi-protocol wireless security auditing.

### Archived (completed; hardware since repurposed)

- **[04 - CYD Marauder + GPS](projects/04-cyd-marauder-gps-archived)** - ESP32 Marauder wardriving/WiFi-recon build. Completed; board later repurposed.
- **[05 - CYD FancyGotchi](projects/05-cyd-fancygotchi-archived)** - Pwnagotchi-style passive WPA handshake/PMKID capture. Completed; board later repurposed.

### Planned

- **Blue-team wireless sensor** - repurposing the dead-touchscreen CYD into a headless deauth/rogue-device detector. *(Not yet built.)*
- **Suricata -> Wazuh detection layer** - network IDS on the host bridge feeding the SIEM for automated attack detection. *(Next build.)*

## Methodology

1. **Objective** - what I'm building/testing and why
2. **Hardware / Software** - what's in the kit
3. **Build Process** - step-by-step with real troubleshooting notes
4. **Results** - what worked, what didn't, evidence
5. **Defensive Takeaways** - how a defender detects or mitigates this

## Legal & Ethics

All testing is performed on hardware and networks I own, in an isolated lab with no connection to external networks. Nothing here targets systems or people I don't have authorization for. This lab exists to understand attacks so I can better detect and defend against them.

## Contact

- GitHub: [@cshd05223-netizen](https://github.com/cshd05223-netizen)
