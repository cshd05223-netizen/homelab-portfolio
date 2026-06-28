# WPA2 Handshake Capture & Crack

> **Status:** Planned

## Objective

Execute the full WPA2 attack chain: capture a 4-way handshake from my own network, transfer the capture file to the GPU-equipped main rig, and crack it with Hashcat. Understand the attack pipeline end-to-end so I can assess wireless password strength and advise on defense.

## Attack Chain Overview

1. **Recon** — Identify target AP (my own network) and connected clients using CYD or wireless adapter
2. **Deauth** — Send deauthentication frames to force a client to reconnect (generates a new handshake)
3. **Capture** — Capture the WPA2 4-way handshake in a .cap/.pcapng file
4. **Convert** — Convert capture to Hashcat-compatible .hccapx format
5. **Crack** — Run Hashcat with GPU acceleration against wordlists (rockyou.txt, custom lists)
6. **Analyze** — Evaluate password strength based on crack time

## Hardware

| Role | Device | Purpose |
|------|--------|---------|
| Capture | HP Laptop + external WiFi adapter (monitor mode capable) | Handshake capture |
| Crack | Main Rig — AMD Ryzen 7 5700X, RX 6650 XT (8GB) | GPU-accelerated hash cracking |
| Target | Home WiFi AP (WPA2) | Authorized target network |

## Tools

- **Capture:** aircrack-ng suite (airmon-ng, airodump-ng, aireplay-ng)
- **Conversion:** cap2hccapx or hcxpcapngtool
- **Cracking:** Hashcat with ROCm (AMD GPU support)
- **Wordlists:** rockyou.txt, custom generated lists

## Operational Security

Even though this is my own network, the workflow treats captured data as if it were client engagement material:
- Capture files stored in encrypted directory
- Cracked credentials securely deleted after verification
- No credentials stored in plaintext long-term

## Build Log

*To be completed during build session.*

## Defensive Takeaways

1. **Password length > complexity** — Hashcat with a GPU can crack short complex passwords faster than long simple ones. Use 16+ character passphrases.
2. **WPA3 mitigates this entirely** — WPA3-SAE uses Simultaneous Authentication of Equals, making offline dictionary attacks impossible. Upgrade if hardware supports it.
3. **Monitor for deauth attacks** — Sudden disconnections of all clients can indicate someone is forcing handshake regeneration. WIDS/WIPS can detect this.
4. **Wordlists are predictable** — If your password is in rockyou.txt or any leaked database, it will be cracked in seconds regardless of "complexity."

## OSCP Relevance

- Wireless attack methodology (directly tested in OSCP wireless labs)
- Hash cracking fundamentals (applies to any captured hash, not just WiFi)
- Tool proficiency: aircrack-ng, Hashcat
- Operational security habits for real engagements
