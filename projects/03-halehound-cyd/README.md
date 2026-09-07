# 03, HaleHound CYD, *Complete*

## Objective
Flash an ESP32 Cheap Yellow Display (CYD) with HaleHound to build a compact, multi-protocol wireless security-auditing tool.

## Hardware / Software
- **Board:** ESP32-2432S028 (2.8" Cheap Yellow Display, CH340 USB)
- **Firmware:** HaleHound (CYD edition)

## Capabilities
HaleHound provides WiFi auditing (packet monitor, deauth detection, probe/scanner tools, Evil Twin/captive-portal testing), Bluetooth/BLE tooling (BLE recon, spoofing/pairing analysis, GATT recon -> clone -> honeypot), and Sub-GHz and 2.4GHz work.

## Build Process
1. Flashed HaleHound to the CYD via the browser flasher / PlatformIO (board target: ESP32-2432S028).
2. Completed touch calibration on first boot.
3. Verified the multi-protocol menu (WiFi / Bluetooth / RF modules) loads and runs.

## Results
Working HaleHound board. Used its BLE tooling to explore a device's GATT structure (services/characteristics) and the honeypot/clone workflow against my own device, a hands-on look at BLE attack surface.

**Note:** The wardriving + Flock/surveillance-detection role this board previously covered has been superseded by the Biscuit C5 (project 07). HaleHound remains active as a dedicated WiFi/BLE learning and experimentation board.

## Defensive Takeaways
- Understanding a device's BLE service/characteristic map is how you assess its attack surface, which characteristics are readable/writable and whether they authenticate.
- BLE spoofing/cloning is primarily a threat at first-pairing against unbonded devices; proper pairing/bonding defeats naive clone-and-lure attacks. Studying the attack teaches the defense (spot rogue/cloned devices, understand why bonding matters).
