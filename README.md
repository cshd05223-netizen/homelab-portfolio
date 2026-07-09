# Cybersecurity Homelab Portfolio

A hands-on security lab built on physical hardware I own. **Blue-team defense is the career path; red-team offense is the hobby that feeds it.** Every project documents methodology, results, and defensive takeaways — no fabricated data.

> **Current phase:** Purple dojo — lab core is built (isolated network, Wazuh SIEM, attack/target VMs). Detection is still manual. Next build: Suricata on the host bridge feeding Wazuh to automate the attack->detect->defend loop.

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
| RF/NFC/Sub-GHz | Flipper Zero | Momentum (custom) | Sub-GHz, NFC, IR, BadUSB, iButton, light BLE |

## Projects

### Current

- **[01 - SIEM Homelab (Wazuh)](projects/01-siem-homelab)** *(flagship, in progress)* - Isolated purple learning lab ("dojo"): Wazuh SIEM, sealed virtual network, Kali attacker, Metasploitable target. Lab core is built; detection layer (Suricata -> Wazuh) is not yet built. Attack -> detect -> defend story is in progress.
- **[02 - Counter-Surveillance Detector (M5 Atom Lite / eye-spy)](projects/02-m5-eyespy-counter-surveillance)** *(complete)* - Passive BLE + WiFi detector for surveillance/tracking devices (ALPR cameras, AirTags, body cams). Defensive privacy tool.
- **[03 - HaleHound CYD](projects/03-halehound-cyd)** *(complete)* - ESP32 Cheap Yellow Display flashed with HaleHound for multi-protocol wireless security auditing.
- **[06 - Flipper Zero (Momentum)](projects/06-flipper-zero)** *(complete — setup; ongoing exploration)* - Multi-protocol RF/wireless/hardware learning tool. Sub-GHz, NFC, IR, BadUSB, iButton, BLE. Flashed with Momentum custom firmware.

### Archived (completed; hardware since repurposed)

- **[04 - CYD Marauder + GPS](projects/04-cyd-marauder-gps-archived)** - ESP32 Marauder wardriving/WiFi-recon build. Completed; board later repurposed.
- **[05 - CYD FancyGotchi](projects/05-cyd-fancygotchi-archived)** - Pwnagotchi-style passive WPA handshake/PMKID capture. Completed; board later repurposed.

### Planned / Roadmap

- **Stage 1 — Suricata detection layer** *(In Progress / Next)* — Suricata (network IDS) on the host bridge feeding Wazuh for automated attack detection. Completes the attack->detect->defend story. The detection layer is **not yet built**.
- **Stage 2 — CYD honeypot** *(Planned)* — dead-touchscreen CYD flashed with ESP32-Honeypot (dagnazty/7h30th3r0n3), webhook alerts into SIEM + Wazuh agent on a real endpoint.
- **Stage 3 — Dedicated blue rig** *(Planned / Future)* — separate used office PC as an always-on Wazuh / Security Onion box, independent of the gaming rig.
- **Stage 4 — Network visibility** *(Planned / Future)* — managed switch with port mirroring or pfSense/OPNsense for real network monitoring ("watchtower").
- **Stage 5 — Enterprise sim** *(Planned / Long-term)* — Active Directory, multiple VMs, VLANs.

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
