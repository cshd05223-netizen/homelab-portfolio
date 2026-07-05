# Project 09 — Blue Sensor CYD (Honeypot / Deauth Detector)

## Status: 📋 PLACEHOLDER — Dead-touch CYD board allocated

## Objective

Repurpose the dead-touchscreen CYD (the FancyGotchi board, once it's retired from passive hunting) into a headless blue-team sensor. This device monitors the local wireless environment for attacks — deauth floods, rogue APs, evil twin attempts — and feeds alerts into the [SIEM](../06-siem-homelab/).

## Hardware

| Component | Details | Status |
|-----------|---------|--------|
| Board | ESP32-2432S028R (CYD, dead touchscreen) | Allocated |
| Firmware | Custom Arduino/ESP-IDF sketch (planned) | TODO |
| Connection | WiFi → SIEM box | Planned |

## Planned Capabilities

- **Deauth detection:** Monitor for 802.11 deauth/disassoc frames targeting the home network
- **Rogue AP detection:** Alert on new SSIDs matching the home network name
- **Evil twin detection:** Compare BSSIDs against known-good AP list
- **Honeypot mode:** Broadcast a decoy AP and log connection attempts
- **SIEM feed:** Push alerts to the SIEM via syslog or HTTP webhook

## Security Hardening Requirements

Since this device is a blue-team sensor, it needs to be hardened against the same attacks it detects:

- TODO: Encrypted communication to SIEM (TLS or WPA3)
- TODO: Firmware signature verification
- TODO: No exposed debug interfaces in production
- TODO: Watchdog timer for crash recovery
- TODO: Secure boot if ESP32 supports it

## Build Process

TODO — document when work begins:
1. Retire FancyGotchi firmware from this board
2. Write custom ESP32 sketch for monitoring mode
3. Implement deauth detection logic
4. Configure SIEM integration (syslog/webhook)
5. Test against own HaleHound attacks
6. Harden and deploy headless

## Results

TODO — no fabricated data. Will document:
- Detection accuracy for deauth floods
- False positive rate for rogue AP alerts
- Alert latency (time from attack to SIEM alert)
- Comparison: what the sensor catches vs what it misses

## Defensive Takeaways

TODO — will cover: wireless IDS concepts, the gap between detection and prevention, and why layered monitoring matters.

## OSCP / Career Relevance

- Blue-team sensor deployment and management
- Custom detection rule development
- SIEM integration from custom hardware
- Understanding the defender's perspective on wireless attacks
- This is the bridge between red-team tools and blue-team career
