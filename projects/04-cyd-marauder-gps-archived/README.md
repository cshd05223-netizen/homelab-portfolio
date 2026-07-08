# 04 - CYD Marauder + GPS - *Archived (completed; board repurposed)*

## Objective
Build an ESP32 Marauder wardriving / WiFi-recon device with an integrated GPS module for geolocated access-point mapping.

## Hardware / Software
- **Board:** ESP32 Cheap Yellow Display (2.8")
- **Firmware:** ESP32 Marauder
- **Add-on:** GPS module for wardriving

## What It Did
WiFi scanning and access-point enumeration, with GPS geotagging to map where networks were seen (wardriving), exportable for mapping.

## Results
Completed and working. This was an earlier build in my progression; the board has since been repurposed for other work, so it's archived here as completed past work rather than an active project.

## Defensive Takeaways
- Wardriving shows how much a network reveals just by broadcasting (SSIDs, signal, location). The defensive lesson: minimize broadcast exposure, and understand that AP location/mapping is trivial for anyone nearby.
