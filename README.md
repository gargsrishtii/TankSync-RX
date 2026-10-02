# TankSync RX — PCB Design

> 2-layer receiver board for a wireless tank monitoring system, built around the ESP32-DevKitC-VE and RYLR998 LoRa module. Designed in KiCad 10.0.

---

## Overview

TankSync RX is the receiver-side PCB of a LoRa-based wireless monitoring system. It receives sensor data transmitted over 868/915 MHz LoRa and processes it via an ESP32 for display and relay control.

**Key features:**
- ESP32-DevKitC-VE as the main controller (Wi-Fi + Bluetooth + dual-core)
- RYLR998 LoRa UART module for long-range wireless communication
- 1.3" OLED display connector (4-pin, I2C)
- WS2812B LED data line with 470Ω series resistor
- 3x status LEDs with 330Ω current-limiting resistors
- JST PH 2.0mm power input connector
- JST XH 3-pin connector for external sensor/data interface
- Berg headers (13-pin + 10-pin) for future relay expansion
- Decoupling capacitors on all power rails
- GND copper pour on both layers

---

## Board Specifications

| Parameter | Value |
|---|---|
| Board size | 80 × 50 mm |
| Layers | 2 (F.Cu + B.Cu) |
| PCB thickness | 1.6 mm |
| Design tool | KiCad 10.0 |
| ERC status | 0 errors, 0 warnings |
| DRC status | 0 errors, 0 unconnected |
| Gerbers | Exported and verified |

---

## Schematic

> *See `/schematic/TankSync_RX_Schematic.pdf` for the full schematic.*

Key design decisions:
- **LORA_RST** tied to ESP32 IO12 with a 10KΩ pull-up (R2) to +3V3, allowing software-controlled LoRa reset
- **R3 (470Ω)** series data resistor from IO32 to LED_DATA line (protects ESP32 GPIO from WS2812B inrush)
- **C6, C7, C8 (100nF SMD)** decoupling capacitors placed close to ESP32 power pins
- **C3, C4 (100µF THT)** bulk decoupling on main power rail — these must remain THT (not changed to SMD)
- **J1 (13-pin) and J2 (10-pin)** Berg headers routed to ESP32 for future relay expansion board

---

## Bill of Materials

| Ref | Qty | Value / Part | Package | Description |
|---|---|---|---|---|
| U1 | 1 | RYLR998 | Castellated 5-pin SMD | REYAX 868/915MHz LoRa UART module |
| U2 | 1 | ESP32-DEVKITC-VE | 38-pin THT dual row | Espressif ESP32 dev board |
| C1 | 1 | 10µF | 0805 SMD | Power rail decoupling |
| C2 | 1 | 47µF | 0805 SMD | Power rail decoupling |
| C3, C4 | 2 | 100µF | Radial THT D5.0mm P2.5mm | Bulk decoupling — must stay THT |
| C6, C7, C8 | 3 | 100nF | 0805 SMD | ESP32 pin decoupling |
| R2 | 1 | 10KΩ | 0805 SMD | LORA_RST pull-up |
| R3 | 1 | 470Ω | 0805 SMD | LED data line series resistor |
| R4, R5, R6 | 3 | 330Ω | 0805 SMD | Status LED current limiters |
| J1 | 1 | Berg 13-pin | PinHeader 1x13 2.54mm | Relay expansion header |
| J2 | 1 | Berg 10-pin | PinHeader 1x10 2.54mm | Relay expansion header |
| J3 | 1 | 1.3" OLED | PinHeader 1x04 2.54mm | OLED display connector |
| J4 | 1 | JST PH 2-pin | JST PH S2B 2.0mm | Power input |
| J5 | 1 | JST XH 3-pin | JST XH B3B 2.5mm | External data connector |
| J6 | 1 | LED Panel | PinHeader 1x04 2.54mm | LED panel connector |

---

## Repository Structure

```
tanksync-pcb/
├── README.md
├── LICENSE
├── schematic/
│   └── TankSync_RX_Schematic.pdf
├── gerbers/
│   └── TankSync_RX_Gerbers.zip
├── bom/
│   └── Project1.csv
└── kicad/
    ├── Project1.kicad_pro
    ├── Project1.kicad_sch
    ├── Project1.kicad_pcb
    └── Project1.net
```

---

## Gerber Files

The `/gerbers/` folder contains the complete Gerber set ready for fabrication:

| File | Layer |
|---|---|
| Project1-F_Cu.gbr | Front copper |
| Project1-B_Cu.gbr | Back copper |
| Project1-F_Silkscreen.gbr | Front silkscreen |
| Project1-B_Silkscreen.gbr | Back silkscreen |
| Project1-F_Mask.gbr | Front solder mask |
| Project1-B_Mask.gbr | Back solder mask |
| Project1-F_Paste.gbr | Front solder paste |
| Project1-B_Paste.gbr | Back solder paste |
| Project1-Edge_Cuts.gbr | Board outline |
| Project1-job.gbrjob | Gerber job file |

Recommended fab services: JLCPCB, PCBWay, OSH Park (all accept this Gerber format directly).

---

## Custom Library Dependency

This design uses two custom KiCad components from my public library:

- `MyLibrary:RYLR998_Castellated_5P` — REYAX RYLR998 LoRa module
- `MyLibrary:ESP32-DEVKITC-VE` — Espressif ESP32-DevKitC-VE dev board

**To open this project in KiCad**, you need to add the custom library first:
👉 [gargsrishtii/kicad-parts-library](https://github.com/gargsrishtii/kicad-parts-library)

Follow the installation instructions in that repo's README before opening the `.kicad_sch` or `.kicad_pcb` files.

---

## Pre-fab Checklist (Open Items)

- [ ] Zip and verify Gerbers in a Gerber viewer before ordering
- [ ] Confirm J4 JST PH part number against supplier stock
- [ ] Measure enclosure inner diameter against board outline

---

## Design Tool

- **KiCad 10.0** (PCB design)
- **EasyEDA** (cross-reference for component verification)

---

## License

This hardware design is released under **CERN-OHL-P v2** (Permissive Open Hardware License).
Free to use, modify, and manufacture — no share-alike requirement.
See [LICENSE](LICENSE) for full terms.

---

*Part of the TankSync wireless monitoring system project.*
*Custom KiCad library: [gargsrishtii/kicad-parts-library](https://github.com/gargsrishtii/kicad-parts-library)*
