# 06 - Flipper Zero (Momentum Firmware) - *Complete (Setup); Ongoing Exploration*

## Objective
Set up a Flipper Zero as a multi-protocol RF/wireless/hardware learning tool. Hands-on experience with Sub-GHz, NFC, infrared, BadUSB, iButton, and BLE protocols — understanding these protocols supports the defensive/blue-team goal (you defend what you understand).

## Hardware / Software
- **Device:** Flipper Zero
- **Firmware:** Momentum (custom), Mainline/stable channel, flashed via momentum-fw.dev web installer
- **Storage:** 4GB microSD (formatted via device)

## Flash Process
1. Inserted microSD; resolved initial "SD Card Not Mounted" error by reseating the card. Formatted via Settings > Storage > Format on-device.
2. Flashed Momentum firmware via the official web installer at `momentum-fw.dev/update` in Chromium (WebSerial). Selected the Mainline (stable) channel, latest version. Flash replaced the shipped firmware cleanly.
3. Serial/USB access worked because the user account was already in the `dialout` group (set up earlier for the M5 Atom Lite flash).

## Capability Areas Explored (all on own devices)
- **Infrared** — universal TV remote + Momentum's expanded IR app catalog (IR Scope signal visualizer, AC-brand remotes, IR Intervalometer, Xbox controller, etc.).
- **NFC (13.56 MHz)** — reading cards; understanding secure vs. legacy card types.
- **Sub-GHz** — capture/replay concepts on own remotes.
- **BadUSB** — HID keystroke injection; ability to write and load custom DuckyScripts.
- **BLE** — light Bluetooth use (the Flipper's weakest area).
- **iButton** — contact-based ID tokens.

## Kit Division (deliberate, non-duplicative tooling)
| Device | Covers |
| --- | --- |
| Flipper Zero | Sub-GHz, NFC, IR, BadUSB, iButton, light BLE |
| HaleHound CYD | WiFi + deep BLE |
| M5 Atom Lite | Passive surveillance detection (eye-spy) |

No overlap — each device is the specialist for its band. Duplicative add-ons (Flipper WiFi module, extra NFC boards) were deliberately declined because existing boards already cover those bands.

## Defensive Takeaways
- Understanding RF and wireless protocols hands-on is foundational for defending against wireless threats. You can't write detection rules for attacks you've never seen.
- Deliberate kit curation (no duplicate capabilities) mirrors real-world budget discipline in security teams.
- Learning the difference between secure and legacy NFC card types informs which systems are defensible and which need replacement.
