# esp32-c3-iot-sensor-board
Custom 4-layer IoT sensor board built around the ESP32-C3, with onboard Li-ion charging, BME280 environmental sensing, ambient light + sound sensors, SPI flash, and microSD storage. Designed end-to-end in KiCad — schematic, routing, and 3D verification.# ESP32-C3 IoT Sensor Board

**A custom 4-layer PCB combining Wi-Fi/BLE connectivity, multi-sensor environmental monitoring, and onboard Li-ion battery management — designed from scratch in KiCad.**

![KiCad](https://img.shields.io/badge/KiCad-10.0.3-345574?style=flat&logo=kicad&logoColor=white)
![MCU](https://img.shields.io/badge/MCU-ESP32--C3--WROOM--02-red)
![Layers](https://img.shields.io/badge/PCB-4--Layer-orange)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-active-brightgreen)

![Board 3D render](assets/images/11-3d-render-isometric.png)

---

## Overview

This repository contains the complete hardware design for a self-contained IoT sensor node built around the **ESP32-C3-WROOM-02** module. The board integrates environmental sensing, audio and light sensing, onboard data storage, and a full single-cell Li-ion charging front end — all on a compact 4-layer PCB designed to be both functional and easy to hand-assemble.

The goal of this project was to go beyond a breadboard prototype and design a production-style board end-to-end: schematic capture, component selection, 4-layer stack-up, routing, and 3D verification — all done independently in KiCad.

## Features

- **Wi-Fi + BLE connectivity** via ESP32-C3-WROOM-02 (RISC-V, 160 MHz)
- **USB-C** for power, charging, and USB-UART programming (no external FTDI needed)
- **Onboard Li-ion charging** with charge/standby status LEDs and thermal regulation
- **3.3 V LDO regulation** for clean, stable sensor supply rail
- **Environmental sensing** — temperature, humidity, and pressure (BME280)
- **Ambient light sensing** via phototransistor
- **Microphone + op-amp gain stage** for audio/sound level sensing
- **32 Mbit SPI flash** for onboard data logging
- **microSD card slot** for expandable local storage
- **I2C OLED header** for real-time on-device data display
- **Auto BOOT/RESET circuitry** for one-click flashing (no manual button sequence)
- **SPI test points** broken out for debugging flash/SD communication

## Hardware Overview

| Subsystem | Key Component | Function |
|---|---|---|
| MCU / Radio | ESP32-C3-WROOM-02-H4 | Wi-Fi + BLE 5.0, application processor |
| USB Interface | CP2102N-A02-GQFN20 | USB-to-UART bridge for programming |
| Battery Charger | NCP73871-2CAI/WL | Single-cell Li-ion charge management + status outputs |
| Voltage Regulator | LML1117MPX-3.3 | 5 V → 3.3 V LDO for sensor/MCU rail |
| USB-C Connector | SS-52400-002 | Power delivery + data |
| Battery Connector | B2B-PH-SN4-TB | 2-pin JST for single-cell LiPo/Li-ion |
| Environmental Sensor | BME280 | Temperature, humidity, pressure (I2C) |
| Light Sensor | TEMT6000X01 | Ambient light phototransistor |
| Audio | MAX4466EXK + electret mic | Adjustable-gain microphone amplifier |
| Flash Storage | W25Q32JVSSIQ | 32 Mbit SPI NOR flash |
| Removable Storage | microSD socket (SPI) | Expandable data logging |
| Display | I2C header | External OLED (SDA/SCL breakout) |

**Power path:** USB-C → NCP73871 charge controller → Li-ion battery (JST) → LML1117 3.3 V LDO → ESP32-C3 and sensor rail, with independent charging/charged/power-good status LEDs.

---

## Gallery

### PCB Layout

2D layout showing full component placement and routing across the 4-layer stack.

![PCB layout — top overview](assets/images/01-pcb-layout-2d.png)
*Top copper + silkscreen overview with all reference designators.*

![PCB layout — labeled functional zones](assets/images/06-pcb-layout-labeled.png)
*Board floor-planned into functional zones: REG, USB, SOUND, LIGHT, FLASH, CHARGING, and BOOT.*

<table>
<tr>
<td><img src="assets/images/07-pcb-front-copper.png" alt="Front copper layer"/><br/><sub>Front copper (F.Cu) layer</sub></td>
<td><img src="assets/images/08-pcb-bottom-copper.png" alt="Bottom copper layer"/><br/><sub>Bottom copper (B.Cu) layer</sub></td>
<td><img src="assets/images/09-pcb-routing-view.png" alt="Routing / track view"/><br/><sub>Full routing / track view</sub></td>
</tr>
</table>

### Schematics

| Sheet | Description |
|---|---|
| [1/4 — Power & Root](assets/images/02-schematic-power-usb.png) | USB-C converter, Li-ion battery charger, 3.3 V LDO, JST battery input, power LED |
| [2/4 — MCU & Storage](assets/images/03-schematic-esp32-usb-sd.png) | ESP32-C3 module, USB-UART bridge, SPI flash, microSD socket, SPI test points |
| [3/4 — Sensors](assets/images/04-schematic-sensors.png) | BME280, ambient light sensor, microphone + amplifier |
| [4/4 — UI & Debug](assets/images/05-schematic-oled-buttons.png) | I2C OLED header, GPIO breakout, auto BOOT/RESET circuit |

![Schematic sheet 1](assets/images/02-schematic-power-usb.png)
![Schematic sheet 2](assets/images/03-schematic-esp32-usb-sd.png)
![Schematic sheet 3](assets/images/04-schematic-sensors.png)
![Schematic sheet 4](assets/images/05-schematic-oled-buttons.png)

### 3D Renders

<table>
<tr>
<td><img src="assets/images/10-3d-render-top.png" alt="3D render top view"/></td>
<td><img src="assets/images/13-3d-render-top-alt.png" alt="3D render top angled"/></td>
<td><img src="assets/images/12-3d-render-bottom.png" alt="3D render bottom view"/></td>
</tr>
</table>

---

## Connector Reference

**J5 — GPIO Breakout**

| Pin | Signal |
|---|---|
| 1 | GND |
| 2 | GPIO19 |
| 3 | GPIO18 |
| 4 | GPIO8 |
| 5 | 3.3V |

**J4 — External OLED / I2C Header**

| Pin | Signal |
|---|---|
| 1 | GND |
| 2 | SCL |
| 3 | SDA |
| 4 | 3.3V |

> ⚠️ Verify pin order against the schematic/silkscreen before wiring external modules.

**SPI Test Points**

| Test Point | Signal |
|---|---|
| TP1 | MOSI |
| TP2 | MISO |
| TP3 | SCLK |
| TP4 | CS (SD) |
| TP5 | CS (Flash) |

---

## Repository Structure

```
.
├── hardware/
│   ├── schematic/          # KiCad schematic (.kicad_sch)
│   ├── pcb/                 # KiCad PCB layout (.kicad_pcb)
│   ├── gerbers/              # Fabrication output (Gerber + drill files)
│   └── bom/                  # Bill of materials (CSV)
├── firmware/
│   └── esp32-c3-sensor-node/ # Firmware source for ESP32-C3
├── docs/
│   └── datasheets/            # Component datasheets
├── assets/
│   └── images/                 # README images (see gallery above)
└── README.md
```

## Getting Started

### 1. Hardware

1. Clone this repository.
2. Open `hardware/pcb/*.kicad_pcb` in **KiCad 10.0.3** (or newer) to view/edit the layout.
3. Generate Gerbers via **File → Fabrication Outputs → Gerbers** or use the pre-exported files in `hardware/gerbers/`.
4. Order the board from your preferred fab house (JLCPCB, PCBWay, etc.) — 4-layer, 1.6 mm, HASL or ENIG finish recommended.
5. Assemble using the BOM in `hardware/bom/`. Reflow or hand-solder SMD components, then solder through-hole connectors (USB-C, JST, headers).

### 2. Firmware

1. Install the [Arduino ESP32 core](https://github.com/espressif/arduino-esp32) or set up ESP-IDF.
2. Open `firmware/esp32-c3-sensor-node/` in your IDE of choice.
3. Select **ESP32C3 Dev Module** as the board target.
4. Connect via USB-C — the onboard auto BOOT/RESET circuit handles flashing without manual button presses.
5. Upload and open the serial monitor at `115200` baud.

---

## Applications

- IoT edge devices for environmental monitoring
- Battery-powered data logging systems
- General-purpose prototyping platform for ESP32-C3 projects

## Roadmap

- [ ] Add enclosure/case design
- [ ] Publish full BOM with supplier links
- [ ] Add example firmware for sensor logging to SD card
- [ ] Power consumption / battery life benchmarking

## Contributing

Issues and pull requests are welcome. If you spot a design improvement or find a routing/footprint issue, please open an issue with details (and a screenshot if it's layout-related).

## License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE) for details.

## Author

**Muhammad Sufyan**
Electrical Engineering, PIEAS
[LinkedIn](#) · [GitHub](#)

