# Cybersecurity Homelab Portfolio

A hands-on security lab built on physical hardware I own. Blue-team defense is the career path; red-team offense is the hobby that feeds it. Every project documents methodology, results, and defensive takeaways, no fabricated data.

**Current phase:** **Virtual SOC**, building a full SOC analyst workflow in the isolated lab. Windows VM endpoint → Wazuh agent + Sysmon → attack it from Kali (Meterpreter) → hunt telemetry → TheHive ticketing. The detection lab (Phase 1 network) and VPS honeypot are complete; this is the endpoint + casework layer on top.

## About Me

Cybersecurity student on a blue-team career track, SOC Analyst → Security Engineer. Night-shift hospital supply chain worker building practical defensive security skills through hands-on lab work. I attack my own isolated lab specifically so I can learn to detect and defend against those attacks (a purple-team method in service of a blue-team career).

- **Career target:** SOC Analyst (entry) → Security Engineer
- **In progress:** CompTIA Security+ (primary cert), CCNA
- **Training platforms:** HackTheBox (SOC Analyst path)
- **Focus areas:** SIEM & log analysis, detection engineering, network security monitoring, wireless/RF security

## Lab Environment

| Role | Hardware | OS / Firmware | Purpose |
|------|----------|---------------|---------|
| Main Rig | AMD Ryzen 7 5700X, RX 6650 XT (8GB), 16GB RAM | Pop!_OS 24.04 | SIEM host, hypervisor, VM lab |
| Attacker VM | (virtual) | Kali 2026.2 | Attacks against the isolated lab |
| Target VM | (virtual) | Metasploitable 2 | Vulnerable victim for attack/detect |
| SIEM | (Docker on host) | Wazuh 4.9.0 | Log analysis, detection, dashboards |
| Network IDS | (host, on lab bridge) | Suricata 7.0.3 | Network detection feeding Wazuh |
| Counter-surveillance | M5 Atom Lite (ESP32-PICO-D4) | eye-spy | Passive BLE/WiFi surveillance detector |
| Wardriving / Flock detection | XIAO ESP32-C5 | Biscuit (DIY) | Dual-band wardriving + surveillance detection, phone-controlled |
| Multi-protocol RF | ESP32 CYD (2.8") | HaleHound | WiFi/BLE/Sub-GHz security auditing |
| RF/NFC/Sub-GHz | Flipper Zero | Momentum (custom) | Sub-GHz, NFC, IR, BadUSB, iButton, light BLE |
| VPS Honeypot | DigitalOcean droplet (Ubuntu 24.04) | Cowrie 3.0.12 + Loki/Grafana | Live internet-facing SSH honeypot + dashboard |

## Projects

### Complete

**01 SIEM Homelab (Wazuh + Suricata)** *COMPLETE*
Isolated purple learning lab ("dojo"): Wazuh SIEM, sealed virtual network, Kali attacker, Metasploitable target, Suricata network IDS. The full attack → detect → defend loop is built and verified: an nmap scan from Kali is detected by Suricata, shipped through Filebeat into the Wazuh indexer, and displayed as alerts in the dashboard with correct source-IP attribution. Includes a real detection-pipeline debugging story (see writeup).

**02 Counter-Surveillance Detector (M5 Atom Lite / eye-spy)** *COMPLETE*
Passive BLE + WiFi detector for surveillance/tracking devices (AirTags, body cams, camera vendor OUIs). Defensive privacy tool.

**03 HaleHound CYD** *COMPLETE*
ESP32 Cheap Yellow Display flashed with HaleHound for multi-protocol wireless auditing (WiFi/BLE/Sub-GHz). Now a learning board; wardriving role superseded by the Biscuit C5 (project 07).

**04 CYD Marauder + GPS** *ARCHIVED* 🗄️
ESP32 Marauder wardriving/WiFi-recon build. Completed; board later repurposed.

**05 CYD FancyGotchi** *ARCHIVED* 🗄️
Pwnagotchi-style passive WPA handshake/PMKID capture. Completed; board later repurposed.

**06 Flipper Zero (Momentum)** *COMPLETE*
Multi-protocol RF/wireless/hardware learning tool. Sub-GHz, NFC, IR, BadUSB, iButton, BLE. Flashed with Momentum firmware.

**07 Biscuit Wardriving Node (XIAO ESP32-C5)** *COMPLETE*
Single ~$8 dual-band ESP32-C5 running DIY Biscuit firmware, phone-controlled over Bluetooth. Handles wardriving and Flock/surveillance detection in one device.

**08 Internet-Facing SSH Honeypot (Cowrie + Grafana)** *COMPLETE*
Real internet-exposed SSH honeypot on a DigitalOcean VPS capturing live attacker credentials, commands, and pivot attempts, hardened with key-only auth, Tailscale private dashboard, and a lightweight Loki + Promtail + Grafana log pipeline. 25k+ real attacks over ~2 weeks, a GeoIP attack map plotting attacker locations on a world map, and a honeytoken bait file separating human attackers from bot noise. Survived a self-inflicted lockout → rebuilt with defense-in-depth (see writeup).

### Active

**Virtual SOC**, *in progress, current*
The real SOC build, entirely in the virtual lab. Brick by brick: Windows VM endpoint → Sysmon (SwiftOnSecurity config) → Wazuh agent → attack from Kali (Meterpreter) → hunt telemetry → TheHive ticketing (alert → ticket → investigate → close). This adds the HOST/endpoint detection layer and the actual analyst casework on top of Phase 1's network detection, making it a real SOC workflow, not just a dashboard.

**Opsec Cleanup**, *in progress*
Digital-footprint reduction across finance/crypto cluster, email architecture consolidation, and data-broker removal. Deletion ≠ burial, broker removal is what actually erases the footprint.

**Kali CyberDeck**, *planning / parts-gathering*
Portable Raspberry Pi 4 (8GB) Kali pentest kit, a travel attacker + second attack source for the home lab. Funding locked to gift money; build gated behind "prove it works as a loose pile first."

**Music Server**, *parked*
Navidrome self-hosted media server, hobby infrastructure project, currently parked.

### Parked (waiting on owning my own network)

**Home Network Watcher**, *parked*
A second, separate SIEM watching the real home LAN (Suricata + EveBox). Re-sequenced to parked because it needs physical network access (router/switch downstairs, can't run ethernet or add a managed switch, house rule = don't break anything). Waits until Colin owns his own network. Virtual SOC covers the same learning (agent, telemetry, investigation) without the hardware.

## Roadmap

The detection lab (Stage 1) and the honeypot (Stage 2) are complete, including the GeoIP attack map and honeytoken. Current build is the Virtual SOC (Stage 3). The Home Network Watcher is parked until physical network access is possible.

| Stage | What | Status |
|-------|------|--------|
| 1 | **Detection lab (Suricata → Wazuh)**, attack → detect → defend loop on the isolated lab. | ✅ Complete |
| 2 | **Honeypot on a VPS**, internet-exposed Cowrie SSH honeypot on DigitalOcean, Loki + Grafana dashboard over Tailscale, GeoIP attack map, honeytoken bait file. 25k+ attacks captured. | ✅ Complete |
| 3 | **Virtual SOC**, full analyst workflow in the lab: Windows VM + Sysmon + Wazuh agent → attack from Kali (Meterpreter) → hunt telemetry → TheHive ticketing → iterate. Endpoint detection + real casework on top of Phase 1. | 🔨 In progress |
| 4 | **Detection exercises**, run an attack, watch it in Wazuh, write a detection rule, learn to defend. Built into the Virtual SOC iteration loop. | Next |
| 5 | **Home Network Watcher**, separate SIEM (Suricata + EveBox) on the real home LAN. Parked until Colin owns his own network (router + managed switch + physical access). | ⏸️ Parked |
| 6 | **Backup systems**, full system/DR backup pass. | Planned |
| 7 | **Finish Opsec cleanup**, complete the digital-footprint reduction project. | Planned |

**Trust-tier discipline:** deliberately-attacked systems (lab, honeypot) are kept isolated from any trusted-device monitoring. The honeypot is meant to be compromised, so it never shares a box or a SIEM brain with anything I actually protect.

## Methodology

- **Objective**, what I'm building/testing and why
- **Hardware / Software**, what's in the kit
- **Build Process**, step-by-step with real troubleshooting notes
- **Results**, what worked, what didn't, evidence
- **Defensive Takeaways**, how a defender detects or mitigates this

## Legal & Ethics

All testing is performed on hardware and networks I own, in an isolated lab with no connection to external networks. Nothing here targets systems or people I don't have authorization for. This lab exists to understand attacks so I can better detect and defend against them.

## Contact

GitHub: [@cshd05223-netizen](https://github.com/cshd05223-netizen)
