# Plantie: ESP32 Low-Power Plant Monitor

A complete open-source ecosystem for smart plant monitoring. This project consists of a battery-powered, low-energy **Control Unit** that interfaces with a soil moisture sensor, and a dedicated **Base Station** for charging and data upload.

## 🚧 Project Status

This project is currently under active development.

- [X] **Control Unit:** Hardware Schematics
- [ ] **Control Unit:** PCB Layout
- [ ] **Control Unit:** Firmware
- [ ] **Control Unit:** 3D Printable Enclosure Design
- [X] **Base Station:** Hardware Schematics
- [ ] **Base Station:** PCB Layout
- [ ] **Base Station:** 3D Printable Enclosure Design

## 📂 Repository Structure

```text
.
├── firmware/              # Source code for the ESP32 Control Unit
│   └── control-unit/      # Code for the main module
│
├── hardware/              # PCB Design files (KiCad)
│   ├── base-station/      # (WIP) Schematic & layout for the dock
│   └── control-unit/      # Schematic & layout for the main module
│
└── mechanical/            # (WIP) 3D models for enclosures

```

## ⚖️ License

This project is dual-licensed to accommodate both hardware and software freedoms:

### 🖥️ Software (Firmware)

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

All code located in the `firmware/` directory is licensed under a [GNU General Public License v3.0 (GPL-3.0)](LICENSE-GPL).

### 🛠️ Hardware (Schematics, PCB, 3D Models)
[![CC BY-SA 4.0][cc-by-sa-shield]][cc-by-sa]

All hardware designs located in the `hardware/` directory are licensed under a
[Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa].

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg