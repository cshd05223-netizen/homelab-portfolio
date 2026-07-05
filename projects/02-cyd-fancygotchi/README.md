# Project 02 — CYD FancyGotchi (Passive Handshake Hunter)

## Status: ✅ COMPLETE + RUNNING — Board earmarked for future [Blue Sensor](../09-blue-sensor-cyd/) conversion

## Objective

Build a standalone passive WPA handshake and PMKID capture device using an ESP32 CYD running FancyGotchi — a Pwnagotchi-style firmware that hunts for authentication material without active attacks.

## Hardware

| Component | Details |
|-----------|---------|
| Board | ESP32-2432S028R (CYD, 2.8", single micro-USB) |
| Firmware | FancyGotchi CYD port |
| Flasher | [AtomNFT FancyGotchi CYD Flasher](https://atomnft.github.io/Fancygotchi-CYD-port-flasher/) |
| SD Card | FAT32 formatted, stores .pcap captures |

## Build Process

1. Used the AtomNFT web flasher — selected "Fancygotchi CYD" build (left column, single micro-USB)
2. Flashed clean, booted to animated face + mood display
3. Inserted FAT32 SD card — mount confirmed
4. Left running overnight at desk for passive collection

## Results

- Collected ~22 handshakes/PMKIDs overnight at desk
- Those ~22 span ALL nearby networks (neighbors included) — only own-network captures are legal to crack-test
- Captures saved as .pcap to SD card
- Screen-zone controls working: ^L theme, ^R face, vL web, vR deauth

### Metrics Meaning

| Metric | What It Means |
|--------|---------------|
| EAPOL | Full WPA 4-way handshake — requires a device to connect/reconnect to the AP |
| PMKID | Clientless capture pulled directly from the AP beacon — no deauth needed |
| Pwned | Total saved captures across both types |

## Defensive Takeaways

- Passive capture requires ZERO interaction with the target — no deauth, no rogue AP, no alerts
- PMKID capture means an attacker doesn't even need to wait for a client to connect
- WPA2-Personal with weak passwords falls to offline cracking after a single PMKID grab
- WPA3-SAE eliminates PMKID attacks entirely — strongest defense against passive capture
- Enterprise networks using 802.1X/RADIUS are not vulnerable to this capture method

## OSCP / Career Relevance

- Understanding passive vs active wireless attacks
- The capture-to-crack pipeline (FancyGotchi feeds into the Hashcat backlog project)
- Legal scoping: identifying which captures are in-scope vs out-of-scope
- Firmware flashing and embedded device management
