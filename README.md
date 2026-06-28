# Cybersecurity Homelab Portfolio

Hands-on offensive security projects built on physical hardware. Each project documents the build process, methodology, tools used, and defensive takeaways — structured to mirror real-world pentest engagement reporting.

> This is a living repo. Projects are added as they're completed in my home lab environment.

## About Me

Cybersecurity student pursuing OSCP certification. Night-shift hospital supply chain worker building practical offensive security skills through hands-on lab work. Currently training via TryHackMe and HackTheBox.

- **Focus Areas:** Wireless security, network pentesting, web application security
- **Certifications In Progress:** OSCP (OffSec Certified Professional)
- **Training Platforms:** TryHackMe, HackTheBox

## Lab Environment

| Role | Hardware | OS | Purpose |
|------|----------|----|---------|
| Main Rig | AMD Ryzen 7 5700X, RX 6650 XT (8GB), 32GB RAM | Pop!_OS 24.04 | Attack platform, hash cracking, VM host |
| Recon Laptop | HP Pavilion i3 | Parrot OS (planned dual-boot) | Wireless recon, portable capture |
| Target Box | HP Stream Laptop | Windows | Authorized victim for network attacks |
| WiFi Recon (Board 1) | ESP32 CYD (2.8" single micro-USB) | FancyGotchi | Passive WPA handshake/PMKID capture |
| WiFi Attack (Board 2) | ESP32 CYD (2.8" single micro-USB) | ESP32 Marauder + GPS | WiFi scanning, Evil Portal, GPS wardriving |
| RF/Hardware | Flipper Zero (incoming) | Flipper FW | Sub-GHz, RFID, NFC, IR, BLE recon |

## Projects

### Completed
- [CYD Marauder + GPS Wardriving Build](projects/01-cyd-marauder-gps/) — ESP32 Marauder with integrated GPS module for WiFi recon and geolocated AP mapping
- [CYD FancyGotchi Passive Hunter](projects/02-cyd-fancygotchi/) — Pwnagotchi-style passive WPA handshake and PMKID capture device

### In Progress
- [Evil Portal](projects/03-evil-portal/) — Captive portal credential capture attack simulation
- [WPA2 Handshake Capture & Crack](projects/04-hashcat-wpa2-crack/) — Full wireless attack chain: capture, transfer, crack with Hashcat + GPU

## Methodology

Every project follows this structure:
1. **Objective** — What am I testing and why?
2. **Hardware/Software** — What's in the kit?
3. **Build Process** — Step-by-step with troubleshooting notes
4. **Results** — What worked, what didn't, evidence
5. **Defensive Takeaways** — How do you defend against this attack?
6. **OSCP Relevance** — How does this skill map to the certification?

## Legal & Ethics

All testing is performed on hardware and networks I own. No unauthorized access is performed. Projects involving credential capture (Evil Portal, WPA2 cracking) use only my own devices and my own network credentials. This lab exists to understand attacks so I can better defend against them.

## Contact

- GitHub: [@cshd05223-netizen](https://github.com/cshd05223-netizen)
