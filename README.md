# Custom ESP32-C3 IoT Sensor Board

A professional, high-reliability 4-layer IoT edge platform designed end-to-end in KiCad. This board consolidates multi-sensor environmental telemetry, secure local storage, and intelligent Li-ion battery management around a RISC-V core.

![KiCad](https://img.shields.io/badge/KiCad-10.0.3-345574?style=flat&logo=kicad&logoColor=white)
![MCU](https://img.shields.io/badge/MCU-ESP32--C3--WROOM--02-red)
![Layers](https://img.shields.io/badge/PCB-4--Layer-orange)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-active-brightgreen)

![Board 3D render](10-3d-front-isometric.png)

---

## Technical Overview

The **ESP32-C3 IoT Sensor Board** is an ultra-compact, production-grade hardware platform built around the **ESP32-C3-WROOM-02** module. Designed to bridge the gap between benchtop breadboard prototypes and ruggedized field deployments, it integrates multi-domain sensing (atmospheric, acoustic, and luminous), redundant high-speed local storage, and a robust power management front-end onto a tightly constrained 4-layer stack-up.

### Subsystem Architecture

| Subsystem | Silicon / Component | Technical Specifications & Role |
| :--- | :--- | :--- |
| **MCU & Radio** | ESP32-C3-WROOM-02-H4 | 32-bit RISC-V single-core processor @ 160 MHz, integrated 2.4 GHz Wi-Fi & BLE 5.0 |
| **USB-UART Bridge**| CP2102N-A02-GQFN20 | High-speed data transit up to 3 Mbps; hardware flow control for automated flashing |
| **Power Path / Charger** | NCP73871-2CAI/WL | Linear Li-ion charge management with programmable thermal regulation & status pins |
| **LDO Regulator** | LML1117MPX-3.3 | High-efficiency 5 V to 3.3 V LDO supplying up to 800 mA to critical RF & sensor rails |
| **Atmospheric Sensing** | BME280 | Ultra-low power digital sensor for temperature, relative humidity, and barometric pressure |
| **Luminous Sensing** | TEMT6000X01 | Silicon NPN phototransistor matching human eye spectral sensitivity for ambient light sensing |
| **Acoustic Sensing** | MAX4466EXK + Mic | Electret condenser microphone paired with a rail-to-rail op-amp boasting adjustable gain |
| **Onboard Storage** | W25Q32JVSSIQ | 32 Mbit SPI NOR Flash dedicated to structured logging, failsafe firmware, or local databases |
| **Removable Storage** | microSD Socket | SPI-driven MicroSD card slot for bulk data logging and local diagnostic storage |

---

## Schematic Design

The design is modularly partitioned across 4 distinct hierarchical sheets to optimize signal routing, isolate analog front-ends from high-frequency RF noise, and simplify debugging:

* **01 — Power & Root Management:** Contains the USB-C power input, the single-cell Li-ion charger circuitry, independent status indicators, and the low-dropout regulator.
* **02 — Core Processing & Storage:** Houses the ESP32-C3 module, decoupling networks, the high-speed USB-UART interface, the local 32 Mbit NOR flash, and the microSD card slot.
* **03 — Analog & Digital Sensors:** Implements the digital I2C BME280, the analog phototransistor network, and the MAX4466 microphone pre-amplifier stage.
* **04 — User Interface & Diagnostics:** Hosts the I2C OLED display breakout, tactile user inputs, general-purpose GPIO expansion headers, and the transistor-based auto-program/reset logic.

![Schematic Sheet 1 - Power & Root](01-schematic-power.png)
![Schematic Sheet 2 - Processing & Storage](02-schematic-esp32-storage.png)
![Schematic Sheet 3 - Sensors](03-schematic-sensors.png)
![Schematic Sheet 4 - User Interface & Diagnostics](04-schematic-oled-debug.png)

---

## PCB Stack-Up & Layout

The board is configured as a high-density, controlled-impedance **4-layer PCB**. The stack-up is strategically layered to establish continuous ground references, minimize EMI/RFI radiation, and maximize power delivery performance.

<table>
  <tr>
    <td align="center"><img src="05-front-copper.png" width="100%" alt="Layer 1: Front Copper"/><br/><sub><b>Layer 1 (Signal / RF Path / Local Power)</b></sub></td>
    <td align="center"><img src="06-inner1-copper.png" width="100%" alt="Layer 2: Inner Ground Plane"/><br/><sub><b>Layer 2 (Solid Ground plane for RF/Signal shielding)</b></sub></td>
  </tr>
  <tr>
    <td align="center"><img src="07-inner2-copper.png" width="100%" alt="Layer 3: Inner Power Plane"/><br/><sub><b>Layer 3 (Divided Power plane for 5V / 3.3V rails)</b></sub></td>
    <td align="center"><img src="08-back-copper.png" width="100%" alt="Layer 4: Back Copper"/><br/><sub><b>Layer 4 (Signal routing)</b></sub></td>
  </tr>
</table>

---

## Mechanical & 3D Verification

A complete 3D model was compiled during the design phase to guarantee strict component-to-component clearances, verify connector pitch tolerances, and ensure seamless fitment within custom hardware enclosures.

<table>
  <tr>
    <td align="center"><img src="09-3d-front-view.png" width="100%" alt="3D Front View"/><br/><sub><b>Front Orthographic Assembly</b></sub></td>
    <td align="center"><img src="11-3d-back-view.png" width="100%" alt="3D Back View"/><br/><sub><b>Back Orthographic Assembly</b></sub></td>
  </tr>
</table>

![3D Isometric View](10-3d-front-isometric.png)
*Perspective render showcasing mechanical profile heights, component density, and silkscreen alignment.*

---

## Pinout and Connector Interfaces

### J4 — External OLED Header (I2C)
| Pin | Physical Designation | Signal Line | Description |
| :---: | :---: | :---: | :--- |
| **1** | GND | Ground | Main system ground reference |
| **2** | SCL | GPIO9 / SCL | I2C Serial Clock (10k pull-up onboard) |
| **3** | SDA | GPIO8 / SDA | I2C Serial Data (10k pull-up onboard) |
| **4** | 3.3V | VCC_3V3 | Cleaned 3.3V system power rail |

### J5 — GPIO Expansion Port
| Pin | Physical Designation | Signal Line | Description |
| :---: | :---: | :---: | :--- |
| **1** | GND | Ground | Main system ground reference |
| **2** | IO19 | GPIO19 | General-purpose digital Input/Output |
| **3** | IO18 | GPIO18 | General-purpose digital Input/Output |
| **4** | IO8 | GPIO8 | General-purpose digital Input/Output |
| **5** | 3.3V | VCC_3V3 | Cleaned 3.3V system power rail |

### High-Speed SPI Diagnostics Test Points
For logic analyzer debug and verification of high-speed flash and SD card communication:

| Test Point | Signal | Target Subsystem |
| :---: | :---: | :---: |
| **TP1** | MOSI | Master Out Slave In |
| **TP2** | MISO | Master In Slave Out |
| **TP3** | SCLK | Serial Clock |
| **TP4** | CS_SD | Chip Select (microSD Card) |
| **TP5** | CS_FL | Chip Select (NOR Flash) |

---

## Key Hardware Features

* **Auto-Program Circuitry:** Integrated dual-transistor automatic reset and bootloader sequence (no physical button combination required during code compilation and deployment).
* **Dual Storage Buses:** Independent Chip Select (`CS`) lanes allow concurrent, conflict-free operations between onboard Flash memory logging and high-capacity MicroSD file systems over a shared SPI bus.
* **Power Isolation:** Features split ground layouts to prevent analog-to-digital crosstalk, especially around the sensitive microphone op-amp stages.

---

## Author

Designed and maintained by:

**Muhammad Sufyan** Electrical Engineering Department  
*Pakistan Institute of Engineering and Applied Sciences (PIEAS)* [LinkedIn](#) · [GitHub](#)
