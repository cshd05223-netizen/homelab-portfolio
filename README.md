# Cybersecurity Homelab Portfolio

A hands-on security lab built on physical hardware. Blue-team defense is the career path; red-team offense is the hobby that feeds it. Every project documents methodology, tools, results, and takeaways — structured like real engagement reporting.

> This is a living repo. Projects are added as they're completed.

## About Me

Cybersecurity student pivoting into a Security Engineer career via SOC work. Night-shift hospital supply chain worker building practical security skills through hands-on lab work.

- **Career Path:** Security Engineer (SOC Analyst → Security Engineer)
- **Certifications In Progress:** Security+ → CCNA → CySA+ (OSCP long-term)
- **Training Platforms:** TryHackMe, HackTheBox
- **Focus Areas:** SIEM / detection engineering, log analysis, incident response, wireless security

## Lab Environment

| Role | Hardware | OS / Firmware | Purpose |
|------|----------|---------------|---------|
| Main Rig | AMD Ryzen 7 5700X, RX 6650 XT (8GB), 32GB RAM | Pop!_OS 24.04 | Attack platform, hash cracking, VM host, SIEM (planned) |
| SIEM Box | Used PC (sourcing) | TBD | Dedicated SIEM — log aggregation, detection engineering |
| Recon Laptop | HP Pavilion i3 | Parrot OS (planned dual-boot) | Wireless recon, portable capture |
| Target Box | HP Stream Laptop | Windows | Authorized victim for network attacks |
| WiFi Recon (Board 1) | ESP32 CYD (2.8" single micro-USB) | FancyGotchi | Passive WPA handshake/PMKID capture |
| WiFi Multi-Tool (Board 2) | ESP32 CYD (2.8" single micro-USB) | HaleHound (replaced Marauder) | WiFi/BLE/SIGINT multi-tool, Evil Portal (GARMR), wardriving |
| Blue-Sensor CYD | ESP32 CYD (ordered) | TBD | Blue-team network sensor (planned) |
| T-Embed | LILYGO T-Embed (ordered) | TBD | Awaiting delivery |
| Atom Lite | M5Stack Atom Lite (ordered) | Flock You (dedicated) | Dedicated Flock ALPR / surveillance detection |
| RF/Hardware | Flipper Zero (incoming July) | Flipper FW | Sub-GHz, RFID, NFC, IR, BLE recon |

## Projects

### Blue Team (Career Track)
- [SIEM Homelab](projects/06-siem-homelab/) — Flagship project: dedicated SIEM box for log aggregation, detection engineering, and incident response **(Planned — Sourcing Hardware)**

### Red Team (Hobby Track)
#### Completed
- [CYD Marauder + GPS Wardriving Build](projects/01-cyd-marauder-gps/) — ESP32 Marauder with GPS for WiFi recon and AP mapping **(Firmware Retired → HaleHound)**
- [CYD FancyGotchi Passive Hunter](projects/02-cyd-fancygotchi/) — Pwnagotchi-style passive WPA handshake and PMKID capture
- [CYD HaleHound Multi-Tool](projects/05-cyd-halehound/) — WiFi/BLE/SIGINT multi-tool replacing Marauder as daily driver

#### In Progress / Planned
- [Evil Portal](projects/03-evil-portal/) — Captive portal credential capture via HaleHound GARMR
- [WPA2 Handshake Capture & Crack](projects/04-hashcat-wpa2-crack/) — Full capture → crack pipeline with Hashcat + GPU

## Methodology

Every project follows this structure:
1. **Objective** — What am I testing and why?
2. **Hardware/Software** — What's in the kit?
3. **Build Process** — Step-by-step with troubleshooting notes
4. **Results** — What worked, what didn't, evidence
5. **Defensive Takeaways** — How do you defend against this attack?
6. **Career Relevance** — How does this skill map to the job?

## Legal & Ethics

All testing is performed on hardware and networks I own. No unauthorized access is performed. Projects involving credential capture (Evil Portal, WPA2 cracking) use only my own devices and my own network credentials. This lab exists to understand attacks so I can better defend against them.

## Contact

- GitHub: [@cshd05223-netizen](https://github.com/cshd05223-netizen)
