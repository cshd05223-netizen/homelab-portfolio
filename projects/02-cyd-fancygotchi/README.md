# Project 02 — CYD FancyGotchi (Passive Handshake Hunter)

## Status: ✅ COMPLETE + RUNNING

## Objective

Build a standalone passive WPA handshake and PMKID capture device using a second ESP32-2432S028R (Cheap Yellow Display) running FancyGotchi firmware — a CYD port of the Pwnagotchi concept. The goal is to passively collect WPA handshake material for offline cracking analysis, understanding the capture side of the WPA2 attack pipeline without any active interference with target networks.

## Hardware

| Component | Details |
|-----------|---------|
| Board | ESP32-2432S028R (2.8" TFT, single micro-USB) |
| Firmware | FancyGotchi CYD Port |
| Flasher | [FancyGotchi CYD Flasher](https://atomnft.github.io/Fancygotchi-CYD-port-flasher/) |
| Storage | MicroSD card for .pcap capture storage |

## Firmware Flash Process

1. Connected CYD via micro-USB
2. Navigated to the FancyGotchi CYD web flasher
3. Selected "Fancygotchi CYD" from the left column (single micro-USB variant)
4. Flashed successfully — device booted clean with animated face on first power-up

## How It Works

FancyGotchi is a Pwnagotchi-style AI that passively monitors WiFi traffic and captures WPA authentication material. It requires zero interaction once powered on — just set it near WiFi networks and let it collect.

### Capture Types

| Metric | What It Means | How It's Captured |
|--------|---------------|-------------------|
| **APs** | Access points detected in range | Passive beacon frame monitoring |
| **EAPOL** | Full WPA 4-way handshakes | Requires a device to connect or reconnect to an AP — captured passively by sniffing the authentication exchange |
| **PMKID** | Clientless WPA capture | Extracted from AP beacon/association frames — no client device needed, no deauth needed |
| **Pwned** | Total saved captures | Running count of all captured authentication material saved to SD |

### Display & Controls

- Animated face with mood indicator (reflects capture activity)
- Screen-zone touch controls:
  - **Top-Left:** Theme toggle
  - **Top-Right:** Face toggle
  - **Bottom-Left:** Web interface
  - **Bottom-Right:** Deauth toggle

### Storage

All captures are saved to the SD card as `.pcap` files, compatible with:
- **Wireshark** — for manual analysis and handshake verification
- **hashcat** — for offline WPA2 cracking (after conversion to `.hccapx` or `.22000` format)
- **aircrack-ng** — alternative cracking tool

## Results

- Device booted clean, SD mounted successfully on first run
- Left running overnight at desk — collected **~22 captures** (mix of EAPOL handshakes and PMKIDs)
- Captures span all nearby networks in range (own network + neighbors' networks detected passively)

## The Capture → Crack Pipeline

This device is the **front half** of the WPA2 offline cracking attack chain:

```
FancyGotchi captures .pcap → Transfer to PC → Filter for OWN network only →
Convert to hashcat format → Crack with hashcat on GPU (RX 6650 XT) →
Verify own password strength
```

**⚠️ CRITICAL: Only own-network captures (Bam0701 / known Comcast BSSID) are legal to crack-test.** The device captures everything in range passively, but cracking a neighbor's handshake without authorization is a federal crime under the CFAA.

## Defensive Takeaways

1. **Passive capture is invisible.** There is no way to detect that a FancyGotchi is collecting your handshakes. It sends nothing — it only listens. This is why strong passwords matter.
2. **WPA2 handshakes are always exposed.** Every time a device connects to your network, the 4-way handshake is broadcast in cleartext. Anyone in RF range can capture it.
3. **PMKID doesn't even need a client.** Some APs leak PMKID in the first frame, meaning an attacker doesn't need to wait for a device to connect or force a deauth.
4. **Password strength is your only defense.** Once captured, the handshake is cracked offline with no rate limiting. A weak password falls in minutes. A strong one (16+ random chars) is computationally infeasible.
5. **WPA3 mitigates this.** SAE (Simultaneous Authentication of Equals) replaces the 4-way handshake and is resistant to offline dictionary attacks.

## OSCP Relevance

- The passive capture → offline crack pipeline is the core WiFi pentest methodology
- Understanding EAPOL vs PMKID capture methods directly maps to wireless module content
- Differentiating legal passive capture from illegal active attacks is essential for real engagement scoping
- Chain continues in [Project 04 — Hashcat WPA2 Crack](../04-hashcat-wpa2-crack/) (pending)

## Legal & Ethical Notes

- All captures are passive — the device transmits nothing and causes zero network disruption
- Passive RF monitoring is legal in the US (receiving publicly broadcast signals)
- Only own-network captures will be used for cracking exercises
- Neighbor network captures are automatically collected but will NOT be cracked or analyzed
