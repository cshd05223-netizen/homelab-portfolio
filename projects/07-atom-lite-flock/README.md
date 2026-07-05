# Project 07 — M5 Atom Lite Flock Detector

## Status: 📋 PLACEHOLDER — Device Inbound (ETA ~Jul 5)

## Objective

Flash the M5 Atom Lite with flock-you firmware to create a dedicated ALPR (Automated License Plate Reader) and surveillance camera detector. The Atom Lite's RGB LED provides visual alerts when Flock Safety cameras or similar systems are detected nearby.

## Hardware

| Component | Details | Status |
|-----------|---------|--------|
| Board | M5 Atom Lite | Ordered (~$17) |
| Firmware | flock-you (Atom-Lite/simeononsecurity fork) | Ready to flash |
| Flash Tool | PlatformIO | TODO |

## Flash Notes

- Use the **Atom-Lite/simeononsecurity fork**, NOT the XIAO-S3 default
- Verify board target in `platformio.ini` before flashing
- RGB LED = alert indicator

## Build Process

TODO — document when device arrives:
1. Install PlatformIO
2. Clone the flock-you repo (simeononsecurity fork)
3. Verify platformio.ini targets Atom Lite board
4. Flash firmware
5. Test detection range and alert behavior

## Results

TODO — no fabricated data. Will document:
- Detection range and reliability
- False positive rate
- LED alert patterns
- Comparison with HaleHound's Flock feature (which crashes the CYD)

## Defensive Takeaways

TODO — will cover surveillance awareness, ALPR network mapping, and privacy implications of automated license plate tracking infrastructure.

## OSCP / Career Relevance

- Firmware flashing and embedded device management
- Understanding surveillance infrastructure from a security perspective
- PlatformIO toolchain experience
