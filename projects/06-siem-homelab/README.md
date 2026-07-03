# Project 06 — SIEM Homelab (Flagship Blue-Team Project)

> **Status:** Planned — Sourcing Hardware

## Objective

Build a dedicated SIEM (Security Information and Event Management) homelab on a separate used PC. Ingest logs from the full lab environment — main rig, CYDs, network devices — detect threats, write detection rules, and build the core blue-team skill set that maps directly to SOC Analyst and Security Engineer roles.

## Hardware

| Role | Device | Status |
|------|--------|--------|
| SIEM Box | Used PC (sourcing) | TODO — sourcing |
| Blue-Sensor CYD | ESP32 CYD (ordered) | TODO — ordered, awaiting arrival |
| Log Sources | Main rig, CYDs, network gear | Existing |

## Planned Stack

*To be determined during build session. Candidates:*
- **Wazuh** — open-source SIEM + EDR + compliance
- **ELK Stack** (Elasticsearch, Logstash, Kibana) — log aggregation + visualization
- **Splunk Free** — industry-standard SIEM (500MB/day free tier)
- **Suricata / Zeek** — network IDS/IPS

## Build Log

*To be completed during build session.*

## Why This Is the Flagship Project

Red-team skills (CYD tools, Hashcat, Evil Portal) show you can attack. Blue-team skills (SIEM, detection engineering, log analysis, incident response) show you can **defend** — which is what Security Engineer and SOC Analyst roles actually hire for. This project bridges the gap: attacks from the red track generate the alerts and logs that the SIEM detects.

## Defensive Takeaways

*To be completed after build — will include detection rule methodology, log source coverage analysis, and gap identification.*

## Career Relevance

- SOC Analyst: log triage, alert investigation, incident timeline reconstruction
- Security Engineer: detection rule authoring, SIEM architecture, log pipeline design
- Security+, CySA+ certification material maps directly to SIEM operations
- Real engagement experience with industry-standard tools

## Legal & Ethical Notes

- All log sources are own devices on own network
- No external network monitoring or third-party data ingestion
