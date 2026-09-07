# 02, Counter-Surveillance Detector (M5 Atom Lite / eye-spy), *Complete*

## Objective
Build a portable, passive detector that flags nearby surveillance and tracking devices, a defensive privacy tool. Learn to *interpret* wireless detections, not just run a scanner.

## Hardware / Software
- **Board:** M5Stack Atom Lite (ESP32-PICO-D4, 4MB flash)
- **Firmware:** eye-spy v1.1 (simeononsecurity), Apache-2.0
- **Toolchain:** PlatformIO on Pop!_OS

## What It Detects
Passively scans BLE + WiFi for surveillance/tracking devices: Flock Safety ALPR cameras, Axon body cameras, AirTags / Tile / SmartTag trackers, drones (OpenDroneID), and surveillance-camera vendor OUIs (Hikvision, Ring, Nest, Wyze, etc.). An onboard RGB LED shows threat level via a confidence score (green = clear, yellow = caution, red flashing = alert). BLE scanning is passive, the device is not detectable by the equipment it watches.

## Build Process
1. Installed PlatformIO (pip + PATH setup), added user to the dialout group for serial access.
2. Cloned eye-spy; built with `pio run -e atom-lite` (PlatformIO auto-installed the ESP32 platform, NeoPixel, and NimBLE libraries).
3. Flashed with `pio run -e atom-lite -t upload` (auto-detected /dev/ttyUSB0, chip confirmed ESP32-PICO-D4, hash verified).
4. Verified live over serial: device booted, scanned, and immediately flagged nearby BLE trackers.

### Note on firmware choice
The original "flock-you" repo only targets XIAO ESP32S3 / LilyGO T-Dongle S3, no Atom Lite build. eye-spy (same author's Atom Lite build) covers Flock + broader surveillance detection and matches this hardware.

## Results
On first run it detected AirTag + persistent-unknown-device signatures. Root cause was ambient Apple devices (old AirPods/headphones) and neighbor devices through walls, expected false positives in a BLE-dense environment, not a real threat. Runs standalone off any USB power with the LED as the readout.

## Defensive Takeaways
- Counter-surveillance detection is a real defensive/privacy skill (spotting trackers, rogue cameras).
- Interpreting detections matters more than collecting them: this tool is most trustworthy in **low-BLE / open environments**, where a detection that follows you is meaningful. In a device-dense home it produces expected false positives.
- The device ignores signals below -90 dBm specifically to cut dense-environment noise, a lesson in tuning a detector to reduce false positives.
