# 05 - CYD FancyGotchi - *Archived (completed; board repurposed)*

## Objective
Build a Pwnagotchi-style passive device that captures WPA handshakes / PMKIDs from nearby WiFi.

## Hardware / Software
- **Board:** ESP32 Cheap Yellow Display (2.8")
- **Firmware:** FancyGotchi

## What It Did
Passively listened for and captured WPA handshake / PMKID material from WiFi in range - the capture half of a WPA2 cracking workflow, done passively.

## Results
Completed and working. An earlier build in my progression; the board has since been repurposed, so it's archived here as completed past work.

## Defensive Takeaways
- Passive handshake/PMKID capture needs no interaction with the target - it just listens. The defensive lesson: WPA2 handshakes can be harvested silently, which is why strong, long passphrases (or WPA3) matter, since capture is only step one before an offline crack.
