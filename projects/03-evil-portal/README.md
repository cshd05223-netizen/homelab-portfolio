# Evil Portal — Captive Portal Attack Simulation

> **Status:** In Progress

## Objective

Deploy a rogue captive portal using the CYD's GARMR Captive Portal feature (HaleHound firmware) to understand how attackers use fake WiFi login pages to harvest credentials. Test against own devices only.

## Attack Chain Overview

1. CYD broadcasts a convincing SSID (e.g., "CoffeeShop_Free_WiFi")
2. Victim device connects to the open AP
3. HTTP traffic is redirected to a captive portal page (index.html on SD card)
4. Portal presents a fake login page (Google sign-in, hotel WiFi terms, etc.)
5. Victim submits credentials thinking it's a legitimate login
6. CYD captures and logs the submitted data

## Hardware

- ESP32 CYD with HaleHound firmware (upgraded from Marauder — see [Project 05](../05-halehound-cyd/))
- MicroSD card with portal files

## Setup

### SD Card Structure
```
/
├── ap.config.txt          # Rogue AP SSID configuration
└── portals/
    └── index.html         # Captive portal login page
```

### Configuration
- `ap.config.txt` — defines the fake access point name
- `index.html` — the credential capture page victims see

## Build Log

*To be completed during build session.*

## Defensive Takeaways

1. **Never enter real credentials on captive portals** — if a "free WiFi" asks you to log in with Google/Facebook, it could be a rogue AP.
2. **Use a VPN on public WiFi** — encrypts traffic even if the AP is malicious.
3. **Verify the network** — ask staff for the exact SSID; rogue APs mimic legitimate ones.
4. **Disable auto-connect** — prevents your device from joining known-name rogue APs automatically.

## OSCP Relevance

- Social engineering methodology
- Rogue AP / man-in-the-middle concepts
- Understanding credential capture techniques to test and defend against them

## Legal Notice

This attack is performed exclusively against my own phone on my own network. Deploying a credential-capture portal against unauthorized targets is a federal crime.
