# Cybersecurity Homelab Portfolio

A hands-on security lab built on physical hardware I own. Blue-team defense is the career path; red-team offense is the hobby that feeds it. Every project documents methodology, results, and defensive takeaways — no fabricated data.

**Current phase:** Detection lab (purple dojo) is **complete and verified** — isolated network, Wazuh SIEM, and a working end-to-end attack → detect pipeline (Kali → Suricata → Wazuh dashboard). **Next build:** a honeypot on a small VPS to collect real internet attack data, paged into my own tooling.

## About Me

Cybersecurity student on a blue-team career track — SOC Analyst → Security Engineer. Night-shift hospital supply chain worker building practical defensive security skills through hands-on lab work. I attack my own isolated lab specifically so I can learn to detect and defend against those attacks (a purple-team method in service of a blue-team career).

- **Career target:** SOC Analyst (entry) → Security Engineer
- **In progress:** CompTIA Security+ (primary cert), CCNA
- **Training platforms:** HackTheBox (SOC Analyst path), TryHackMe
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

## Projects

### Current / Active

**01 — SIEM Homelab (Wazuh + Suricata)** *(flagship — COMPLETE)*
Isolated purple learning lab ("dojo"): Wazuh SIEM, sealed virtual network, Kali attacker, Metasploitable target, Suricata network IDS. The full attack → detect → defend loop is built and verified: an nmap scan from Kali is detected by Suricata, shipped through Filebeat into the Wazuh indexer, and displayed as alerts in the dashboard with correct source-IP attribution. Includes a real detection-pipeline debugging story (see writeup).

**02 — Counter-Surveillance Detector (M5 Atom Lite / eye-spy)** *(complete)*
Passive BLE + WiFi detector for surveillance/tracking devices (AirTags, body cams, camera vendor OUIs). Defensive privacy tool. Retained as the dedicated passive/counter-surveillance node.

**03 — HaleHound CYD** *(complete)*
ESP32 Cheap Yellow Display flashed with HaleHound for multi-protocol wireless auditing (WiFi/BLE/Sub-GHz). Now a learning/experimentation board; its wardriving + Flock role was superseded by the Biscuit C5 (project 07).

**06 — Flipper Zero (Momentum)** *(complete — setup; ongoing exploration)*
Multi-protocol RF/wireless/hardware learning tool. Sub-GHz, NFC, IR, BadUSB, iButton, BLE. Flashed with Momentum custom firmware.

**07 — Biscuit Wardriving Node (XIAO ESP32-C5)** *(complete)*
Single ~$8 dual-band ESP32-C5 running DIY Biscuit firmware, controlled entirely from my phone over Bluetooth (GPS sourced from the phone). Handles wardriving and Flock/surveillance detection in one phone-controlled device — consolidating what previously took multiple boards.

### Archived (completed; hardware since repurposed)

**A1 — CYD Marauder + GPS** — ESP32 Marauder wardriving/WiFi-recon build. Completed; board later repurposed.

**A2 — CYD FancyGotchi** — Pwnagotchi-style passive WPA handshake/PMKID capture. Completed; board later repurposed.

## Roadmap

The detection lab (Stage 1) is done, so the roadmap now moves outward — from a staged home lab toward real-world attack data and automation.

| Stage | What | Status |
|-------|------|--------|
| 1 | **Detection lab (Suricata → Wazuh)** — attack → detect → defend loop on the isolated lab. | ✅ Complete |
| 2 | **Honeypot on a VPS** — a small, isolated, internet-exposed disposable box that catches real attackers, generating real attack data and cases to investigate. | Next |
| 3 | **Honeypot → pager** — wire honeypot alerts into my own notification tooling so real detections page me in real time (SOAR-style alerting). | Planned |
| 4 | **Tor-detection exercise** — run Tor in an isolated VM and build detection for its traffic signature in Wazuh (defensive/legal). | Planned |
| 5 | **Self-hosted services + hardening** — e.g. a self-hosted media server over Tailscale, folded into a 3-2-1 backup/DR plan, as a practical infra + hardening exercise. | Planned |
| 6 | **Host / endpoint detection** — Wazuh agents on my own devices for host-based (endpoint) detection, kept separate from the deliberately-attacked honeypot tier. | Later |

**Trust-tier discipline:** deliberately-attacked systems (lab, honeypot) are kept isolated from any trusted-device monitoring. The honeypot is meant to be compromised — so it never shares a box or a SIEM brain with anything I actually protect.

## Methodology

- **Objective** — what I'm building/testing and why
- **Hardware / Software** — what's in the kit
- **Build Process** — step-by-step with real troubleshooting notes
- **Results** — what worked, what didn't, evidence
- **Defensive Takeaways** — how a defender detects or mitigates this

## Legal & Ethics

All testing is performed on hardware and networks I own, in an isolated lab with no connection to external networks. Nothing here targets systems or people I don't have authorization for. This lab exists to understand attacks so I can better detect and defend against them.

## Contact

GitHub: [@cshd05223-netizen](https://github.com/cshd05223-netizen)
