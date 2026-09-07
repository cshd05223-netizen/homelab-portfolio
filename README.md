# Cybersecurity Homelab Portfolio

I break my own stuff so I can learn how to defend it. Blue team is the career, red team is how I understand what I'm actually defending against. Everything here runs on hardware I own, inside an isolated lab, and every project documents what I did honestly, including the parts that went wrong.

**Right now I'm building a Virtual SOC.** A Windows VM as a monitored endpoint, Wazuh agent plus Sysmon on it, attacking it from Kali with Meterpreter, hunting the telemetry, and running the whole thing through TheHive ticketing so it's a real analyst workflow instead of just another dashboard.

## About Me

Cybersecurity student on the blue team track, heading for SOC Analyst and eventually Security Engineer. I work night shift in a hospital supply chain and spend the rest of my time building practical defensive skills. I attack my own isolated lab specifically so I can learn to detect and defend against those attacks. Purple team method, blue team career.

- **Target:** SOC Analyst (entry), then Security Engineer
- **Studying:** CompTIA Security+, then CCNA
- **Training:** HackTheBox SOC Analyst path
- **Focus:** SIEM and log analysis, detection engineering, network security monitoring, wireless/RF security

## Lab Environment

| Role | Hardware | OS / Firmware | Purpose |
|------|----------|---------------|---------|
| Main Rig | AMD Ryzen 7 5700X, RX 6650 XT (8GB), 16GB RAM | Pop!_OS 24.04 | SIEM host, hypervisor, VM lab |
| Attacker VM | (virtual) | Kali 2026.2 | Attacks against the isolated lab |
| Target VM | (virtual) | Metasploitable 2 | Vulnerable victim for attack/detect |
| SIEM | (Docker on host) | Wazuh 4.9.0 | Log analysis, detection, dashboards |
| Network IDS | (host, on lab bridge) | Suricata 7.0.3 | Network detection feeding Wazuh |
| Counter-surveillance | M5 Atom Lite (ESP32-PICO-D4) | eye-spy | Passive BLE/WiFi surveillance detector |
| Wardriving / Flock detection | XIAO ESP32-C5 | Biscuit (DIY) | Dual-band wardriving plus surveillance detection, phone-controlled |
| Multi-protocol RF | ESP32 CYD (2.8") | HaleHound | WiFi/BLE/Sub-GHz security auditing |
| RF/NFC/Sub-GHz | Flipper Zero | Momentum (custom) | Sub-GHz, NFC, IR, BadUSB, iButton, light BLE |
| VPS Honeypot | DigitalOcean droplet (Ubuntu 24.04) | Cowrie 3.0.12 + Loki/Grafana | Live internet-facing SSH honeypot and dashboard |

## Projects

### Done

**01 — SIEM Homelab (Wazuh + Suricata)**

Isolated purple-team lab. Wazuh SIEM, a sealed virtual network, Kali as the attacker, Metasploitable as the target, Suricata as the network IDS. The full attack to detect to defend loop is built and verified: an nmap scan from Kali gets caught by Suricata, shipped through Filebeat into the Wazuh indexer, and shows up as alerts in the dashboard with correct source IP attribution. Includes a real debugging story where a stale password in the config broke the whole pipeline and I had to trace it down. [See writeup](projects/01-siem-homelab/README.md)

**02 — Counter-Surveillance Detector (M5 Atom Lite / eye-spy)**

Passive BLE and WiFi detector for tracking and surveillance devices: AirTags, body cams, camera vendor OUIs. Defensive privacy tool.

**03 — HaleHound CYD**

ESP32 Cheap Yellow Display flashed with HaleHound for multi-protocol wireless auditing across WiFi, BLE, and Sub-GHz. Now a learning board; the wardriving role moved to the Biscuit C5 (project 07).

**04 — CYD Marauder + GPS** *(archived, hardware repurposed)*

ESP32 Marauder wardriving and WiFi-recon build. Completed, then the board got reused for something else.

**05 — CYD FancyGotchi** *(archived, hardware repurposed)*

Pwnagotchi-style passive WPA handshake and PMKID capture. Completed, board later repurposed.

**06 — Flipper Zero (Momentum)**

Multi-protocol RF and wireless learning tool. Sub-GHz, NFC, IR, BadUSB, iButton, BLE. Flashed with Momentum firmware.

**07 — Biscuit Wardriving Node (XIAO ESP32-C5)**

A single ~$8 dual-band ESP32-C5 running DIY Biscuit firmware, controlled from my phone over Bluetooth. Handles wardriving and Flock/surveillance detection in one device.

**08 — Internet-Facing SSH Honeypot (Cowrie + Grafana)**

A real internet-exposed SSH honeypot on a DigitalOcean VPS that captures live attacker credentials, commands, and pivot attempts. Hardened with key-only auth, a Tailscale-only dashboard, and a lightweight Loki plus Promtail plus Grafana log pipeline. Pulled in 25k+ real attacks over about two weeks, built a GeoIP attack map that plots attacker locations on a world map, and planted a honeytoken bait file to separate actual humans from bot noise. I locked myself out of my own box partway through, diagnosed it as a classic honeypot misconfiguration, and rebuilt it with defense-in-depth so it couldn't happen again. [See writeup](projects/08-vps-honeypot/README.md)

### Active

**Virtual SOC** *(in progress, current focus)*

The real SOC build, entirely inside the virtual lab. Brick by brick: Windows VM endpoint, Sysmon with the SwiftOnSecurity config, Wazuh agent, attack it from Kali with Meterpreter, hunt the telemetry, then wire up TheHive ticketing so an alert turns into a ticket, an investigation, a writeup, and a close. This adds the endpoint detection layer and the actual analyst casework on top of the network detection from project 01. The point is to run the full analyst workflow, not just stand up a dashboard. [See writeup](projects/09-virtual-soc/README.md)

**Opsec Cleanup** *(in progress)*

Digital footprint reduction. Finance and crypto account consolidation, email architecture cleanup, data broker removal. The lesson here is that deletion isn't the same as burial: removing yourself from data brokers is what actually erases the footprint.

### Parked

**Home Network Watcher**

A second, separate SIEM that would watch the real home LAN with Suricata and EveBox. Parked because it needs physical network access I don't have right now: the router and switch are downstairs, I can't run ethernet or add a managed switch, and the house rule is don't break anything. This waits until I have my own network. The Virtual SOC covers the same learning (agent, telemetry, investigation) without the hardware.

**Kali CyberDeck** *(planning)*

A portable Raspberry Pi 4 Kali box, meant to be a travel attacker and a second attack source for the lab. Funding is locked to gift money, and the build is gated behind proving it works as a loose pile of parts before I bother with a case.

**Music Server** *(parked)*

Navidrome self-hosted media server. Hobby project, currently parked.

## Roadmap

| Stage | What | Status |
|-------|------|--------|
| 1 | Detection lab (Suricata to Wazuh): attack to detect to defend loop on the isolated lab | Done |
| 2 | Honeypot on a VPS: Cowrie, Loki + Grafana, GeoIP map, honeytoken, 25k+ attacks captured | Done |
| 3 | Virtual SOC: Windows VM + Sysmon + Wazuh agent, Meterpreter attack, telemetry hunt, TheHive ticketing | In progress |
| 4 | Detection exercises: attack, watch it in Wazuh, write a detection rule, learn to defend (built into the Virtual SOC loop) | Next |
| 5 | Home Network Watcher: separate SIEM on the real home LAN (parked until I own my own network) | Parked |
| 6 | Backup systems: full system and DR backup pass | Planned |
| 7 | Finish Opsec cleanup | Planned |

Trust-tier discipline: deliberately attacked systems (the lab, the honeypot) stay isolated from anything I actually protect. The honeypot is supposed to get compromised, so it never shares a box or a SIEM brain with real devices.

## How I Document

Every project follows the same shape: what I'm building and why, what's in the kit, the build process with real troubleshooting notes, results with evidence, and the defensive takeaway for each thing I attack.

## Legal

All testing happens on hardware and networks I own, in an isolated lab with no connection to anything I'm not authorized to touch. This exists so I can understand attacks well enough to detect and defend against them.

## Contact

GitHub: [@cshd05223-netizen](https://github.com/cshd05223-netizen)
