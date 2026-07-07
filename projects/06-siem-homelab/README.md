# Project 06 — SIEM Homelab (Flagship Blue-Team Project)

## Status: 📋 PLANNED — Architecture Designed, Sourcing Hardware

> This is the flagship project. Everything else in the lab — the red-team boards, the passive hunter, the sensors — exists to generate attacks for the defensive side to detect. The red pieces feed the blue work.

## Objective

Build a self-contained security lab to practice the **attack → detect → defend** loop for SOC analyst / blue-team skill building. Everything runs on hardware and networks Colin owns. Nothing touches the public internet or the home LAN once sealed.

**Career framing:** Blue team is the goal (SOC Analyst → Security Engineer). The attacker (Kali) and target (Metasploitable) exist *to generate attacks for the defensive side to detect* — this is a purple-team *method* in service of a blue-team *career* [8].

## Architecture

The lab runs on the **main rig** (Ryzen 7 5700X, 16GB RAM, RX 6650 XT) using VMs until the dedicated SIEM box is sourced. Once the used PC arrives, the SIEM stack moves to dedicated hardware.

### VM Layout (planned)

| VM | Role | OS |
|----|------|----|
| Kali | Attacker — generates red-team traffic | Kali Linux |
| Metasploitable | Target — vulnerable box to attack | Metasploitable 2/3 |
| SIEM | Log aggregation, detection, alerting | Security Onion / Wazuh |

### Host Security Layer

- **Mullvad VPN** on the host, hardened [8]:
  - Kill switch ON, auto-connect ON, launch-on-startup ON
  - Local network sharing ON (needed so the VM bridge works)
  - Quantum-resistant ON
  - DNS content blockers (ads/trackers/malware) ON
  - Lockdown mode intentionally OFF (can interfere with VM bridge)

## Dedicated Hardware (planned)

| Component | Details | Status |
|-----------|---------|--------|
| SIEM Box | Used office PC — i5 6th-gen+, 16GB RAM, 256GB SSD | Sourcing (~$130) |
| Candidates | Dell Optiplex / HP EliteDesk / Lenovo ThinkCentre | Shopping |

## Software Stack Candidates

| Option | Role | Notes |
|--------|------|-------|
| Security Onion | Full NIDS + SIEM + log management | Heaviest, most complete |
| Wazuh | HIDS + SIEM + compliance | Lighter, good for endpoint focus |
| ELK Stack | Log aggregation + visualization | Flexible, pairs with other tools |
| Splunk Free | SIEM + dashboards | 500MB/day limit, but industry-standard skills |
| Suricata | Network IDS/IPS | Can feed into any of the above |

## Telemetry Sources (from this lab)

| Source | What It Generates |
|--------|-------------------|
| HaleHound CYD | Deauth attacks, evil portal attempts, BLE spam — red-team noise |
| FancyGotchi | Passive handshake captures — baseline wireless activity |
| Blue Sensor CYD | Deauth detection, rogue AP alerts, honeypot hits |
| Target Box (Windows) | Endpoint logs, Sysmon events, process creation |
| Main Rig | Host IDS logs, network tap data |
| Flipper Zero | Sub-GHz, NFC, BLE attack telemetry |

## Evidence & Logging Convention

Per session, two paired files in `~/homelab-lab-notebook/sessions/` [8]:

| File | Purpose |
|------|---------|
| `YYYY-MM-DD-HHMM-short-description.log` | Raw `script` capture (terminal recording) |
| `YYYY-MM-DD-HHMM-short-description.md` | Curated writeup (what happened, what was detected, what was missed) |

- Master index table lives in `~/homelab-lab-notebook/attack-detection-log.md` (running summary across all sessions; the per-session files are the detail) [8]
- Kept lightweight — two commands + a text file. The manual writing IS the learning (detection-engineering reflection) and the hand-done evidence trail is portfolio-credible [8]

## Planned Workflow

1. Source and set up the SIEM box (or start on main rig VMs)
2. Install chosen SIEM stack
3. Configure log ingestion from all lab devices
4. Set up Mullvad VPN host security layer
5. Run red-team attacks from HaleHound / Kali VM
6. Write detection rules that catch those attacks
7. Build dashboards showing attack patterns
8. Document the full attack → detect → alert → investigate cycle
9. Per-session paired .log + .md evidence files

## Results

TODO — this is the project where real results matter most. The writeup will document actual detection rules written, alerts triggered, false positive rates, and investigation workflows. No fabricated data.

**The goal:** ONE complete attack → detection story, documented end to end. That single finished story beats every empty placeholder combined for landing a SOC role [6].

## Defensive Takeaways

TODO — will cover: what attacks are easy vs hard to detect, tuning alert thresholds, dealing with false positives, and how the attack tools in this lab map to real-world threat actor TTPs.

## OSCP / Career Relevance

- SOC analyst core skill: log analysis, alert triage, investigation
- SIEM administration and rule writing
- Understanding attack telemetry from the attacker's perspective (because I built the red-team tools)
- Maps directly to Security+, CySA+, and SOC Analyst job requirements
- This project IS the resume differentiator — building detection for attacks I generated myself
- The evidence/logging convention mirrors real engagement reporting standards
