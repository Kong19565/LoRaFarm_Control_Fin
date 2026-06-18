# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

LoRaFarm Control is a university capstone project (STI 2026, Chitralada) — a portable IoT farm monitoring and control device using ESP32-S3 + SX1262 LoRa transceiver. This repository contains the **interactive UI demo** and project documentation; the embedded firmware (C/C++) is not stored here.

The UI is Thai-language throughout.

## Running the Demo

No build step. Open `demo/index.html` directly in any modern browser.

```
# Windows
start demo/index.html

# Or drag-drop the file into a browser
```

## Architecture: `demo/index.html`

A single ~1,150-line standalone HTML5/CSS3/Vanilla JS file that simulates the device's touchscreen interface. No external dependencies except Google Fonts (Sarabun).

**4-screen navigation system** (bottom nav bar with `showPage(id)`):**

| Screen | ID | Purpose |
|--------|----|---------|
| Status | `status` | Battery %, RSSI signal, farm summary stats |
| Farm Control | `farm` | Zone A/B/C sensor readings + actuator toggles (solenoid valves, pump) |
| Signal Map | `map` | Canvas-drawn concentric ring map (3/8/15 km) with animated node positions |
| Communication | `chat` | Device-to-device messaging + emergency SOS broadcast |

**Key JS patterns:**
- `setInterval`-based live simulation: battery drain, signal variation, current draw — runs every 10 seconds
- Zone switching via `switchZone(zone)` updates sensor display values
- Canvas map in `drawMap()` — redraws animated rings and node labels (A1–D1, GW)
- SOS button triggers `sosAlert()` which flashes LED indicator and appends broadcast message to chat
- All UI state is in-memory (no localStorage, no network calls)

## Hardware Context (for UI/firmware alignment)

- **MCU**: ESP32-S3
- **Radio**: SX1262 LoRa (915 MHz / 868 MHz)
- **Battery**: 3.7V 4000mAh Li-ion + optional solar
- **Sensors**: Soil moisture, temperature, humidity, light intensity
- **Actuators**: 2× solenoid valves, 1× water pump
- **Network topology**: Star — field nodes (A1–D1) report to gateways (GW)
- **IP rating**: IP65 (outdoor weatherproof)

## Documentation

| File | Contents |
|------|----------|
| `Document/LoRaFarm_Control_4.pdf` | 11-page project spec and hardware design |
| `Document/document_pdf.pdf` | 12-page supplementary report |
| `Document/Device_Power_Comparison.xlsx` | Power consumption vs Wi-Fi / NB-IoT benchmarks |
