# Project 06 — SIEM Homelab (Flagship Blue-Team Project)

## Status: 📋 PLANNED — Sourcing Hardware

> This is the flagship project. Everything else in the lab — the red-team boards, the passive hunter, the sensors — exists to generate telemetry that flows into this SIEM for detection, alerting, and incident response practice.

## Objective

Build a dedicated SIEM (Security Information and Event Management) homelab that ingests real attack telemetry from the lab's red-team devices and blue-team sensors. Practice writing detection rules, investigating alerts, and building dashboards — the core SOC analyst workflow.

## Hardware (planned)

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

## Planned Workflow

1. Source and set up the SIEM box
2. Install chosen SIEM stack
3. Configure log ingestion from all lab devices
4. Run red-team attacks from HaleHound / FancyGotchi
5. Write detection rules that catch those attacks
6. Build dashboards showing attack patterns
7. Document the full attack → detect → alert → investigate cycle

## Results

TODO — this is the project where real results matter most. The writeup will document actual detection rules written, alerts triggered, false positive rates, and investigation workflows. No fabricated data.

## Defensive Takeaways

TODO — will cover: what attacks are easy vs hard to detect, tuning alert thresholds, dealing with false positives, and how the attack tools in this lab map to real-world threat actor TTPs.

## OSCP / Career Relevance

- SOC analyst core skill: log analysis, alert triage, investigation
- SIEM administration and rule writing
- Understanding attack telemetry from the attacker's perspective (because I built the red-team tools)
- Maps directly to Security+, CySA+, and SOC Analyst job requirements
- This project IS the resume differentiator — building detection for attacks I generated myself
