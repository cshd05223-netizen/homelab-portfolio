# GPS Wardriving Module

## Objective

Integrate a GPS module with the CYD Marauder to enable geolocated WiFi network mapping (wardriving). Log GPS coordinates alongside WiFi scan data to produce maps of wireless network coverage, encryption types, and signal strength by location.

## Hardware

| Component | Model | Notes |
|-----------|-------|-------|
| GPS Module | BN-220 (or compatible NMEA GPS) | UART serial, connects to CYD RX/TX pins |
| Microcontroller | ESP32 CYD (from Project 01) | Same unit, GPS adds capability |
| Wiring | Dupont jumper wires | GPS TX → CYD RX, shared GND and VCC |

## Build Process

### Step 1: GPS Wiring

Connected the GPS module to the CYD's available GPIO pins:

| GPS Pin | CYD Pin | Function |
|---------|---------|----------|
| TX | RX (GPIO) | GPS data → CYD |
| VCC | 3.3V | Power |
| GND | GND | Ground |

**Troubleshooting note:** Initial GPS fix took several minutes outdoors. Cold start GPS modules need clear sky view — won't lock indoors reliably.

### Step 2: Marauder GPS Configuration

In the Marauder menu:
- Navigated to **GPS** section
- Enabled GPS data input
- Verified NMEA sentence parsing (GGA, RMC strings showing lat/long/altitude/speed)

### Step 3: Wardrive Scanning

With GPS locked:
- Ran WiFi AP scans while moving — Marauder logs each detected AP with its GPS coordinates
- Data saved to SD card in a format compatible with mapping tools

## Results

Successfully captured geolocated WiFi data including:
- SSID, BSSID, encryption type, channel, signal strength
- Latitude/longitude coordinates at time of detection
- Timestamp for each observation

Data can be exported and plotted on a map to visualize wireless network density, identify open networks, and assess coverage gaps.

## Defensive Takeaways

1. **Your network's physical footprint is mappable** — Anyone driving by with a $30 setup (CYD + GPS) can log your network's location, encryption type, and signal reach. **Mitigation:** Use WPA3 where possible, reduce transmit power if coverage allows.

2. **Wardriving reveals patterns** — Repeated passes show which networks are always on, which devices come and go, and signal boundaries. This is passive recon that leaves zero forensic trace on the target network.

3. **WEP/Open networks are targets** — Wardriving data highlights low-hanging fruit. Any network not running WPA2/WPA3 will be prioritized by an attacker. **Mitigation:** Audit your encryption, retire legacy devices that only support WEP.

## OSCP Relevance

- Physical-layer reconnaissance methodology
- Understanding of wireless attack surface from a geographic perspective
- Data collection and correlation skills (combining WiFi + GPS datasets)
- Demonstrates initiative in hardware integration beyond standard toolkits

## Tools Used

- ESP32 Marauder firmware (GPS-enabled build)
- BN-220 GPS module
- CYD ESP32 hardware
- SD card for data logging
