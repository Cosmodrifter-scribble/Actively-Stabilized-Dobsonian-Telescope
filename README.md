# Actively Stabilized Dobsonian Telescope

> An 8" f/5 GPS-controlled motorized Dobsonian telescope built completely from scratch — with closed-loop tracking, active stabilization, and Stellarium control. Built by Vinayak Soni, Indore, India.

---

## What is this?

This is a fully custom-built 8" Newtonian reflecting telescope on a motorized Dobsonian alt-azimuth mount. It started as a simple tracking telescope and evolved into something significantly more capable — a GPS-aligned, actively stabilized, astrophotography-ready instrument built for under ₹55,000 (~$580 USD).

A Celestron NexStar 8" costs over $2,000, lacks most of these features, and you'd never understand how it works. This one I built myself, and I understand every single part of it.

---

## Why I built this

I wanted to understand how modern telescopes actually work — not just use one. That meant building one from scratch, designing every subsystem, and solving every problem myself. Along the way the project grew from "make a basic tracker" into a full closed-loop stabilized GoTo system. The constraint of being in India — where quality optics are hard to find and Aliexpress is banned — pushed me to be creative and resourceful in ways I wouldn't have been otherwise.

---

## How to use it

1. Power on via the main switch — ESP32 boots, TMC2209 drivers initialize
2. Wait 30–60 seconds for NEO-M8N GPS to acquire satellite lock (blue LED turns on)
3. Select a target using the D-pad + OLED menu, or connect Stellarium on your phone over WiFi and click any object
4. Telescope slews to target automatically — both AZ and ALT axes move simultaneously
5. Sidereal tracking begins automatically — object stays centered as Earth rotates
6. Active stabilization via MPU9250 IMU corrects for vibration and wind at 200Hz
7. For astrophotography — attach DSLR via T-ring to the 2" Crayford focuser

---

## Features

| Feature | Details |
|---|---|
| GPS alignment | NEO-M8N — location + time on boot, no manual star alignment |
| Active stabilization | MPU9250 IMU at 200Hz — corrects vibration, wind shake, bumps |
| Closed-loop tracking | 1000PPR optical encoders on both axes — catches missed steps |
| Silent motors | TMC2209 at 32× microstepping — zero vibration |
| GoTo | Stellarium over WiFi (LX200 protocol) + built-in object catalogue |
| AZ accuracy | 360T ring gear — 0.003° per step |
| ALT accuracy | 120T GT2 belt — 0.009° per step |
| Dual core | ESP32-S3 — tracking on Core 0, UI/WiFi on Core 1 |

---

## Specifications

### Optics

| Parameter | Value |
|---|---|
| Primary mirror | GSO 8" f/5 parabolic, 203mm aperture, 1000mm focal length |
| Secondary mirror | GSO 50mm elliptical flat, 1/12 wave |
| Focuser | 2" Crayford (Marcos ATM design, modified for 228.6mm tube) |
| Limiting magnitude | ~14.5 |
| Magnification range | 31× to 250× |

### Tube

| Parameter | Value |
|---|---|
| Tube material | Aluminium |
| Tube OD | 228.6mm |
| Tube ID | 218.6mm |
| Total length | 1100mm |
| Spider position from top | 110mm |
| Secondary position from top | 154mm |
| Primary mirror position from top | 1000mm |
| Secondary offset | 2.26mm toward primary |

### Mount

| Parameter | Value |
|---|---|
| Mount type | Dobsonian alt-azimuth rocker box |
| Rocker box material | 12mm plywood (laser cut) |
| Side panels | 350×680mm |
| AZ bearing | 12" lazy susan + Teflon pads |
| ALT bearing | 25mm aluminium pipe + 2× 6205-2RS bearings |

### Drive System

| Axis | System | Ratio | Steps/rev | Accuracy |
|---|---|---|---|---|
| AZ | 360T Module 1 spur gear ring + 20T pinion | 18:1 | 115,200 | 0.003°/step |
| ALT | 120T GT2 belt + 20T pinion | 6:1 | 38,400 | 0.009°/step |

### Electronics

| Component | Spec |
|---|---|
| Controller | ESP32-S3-DevKitC-1-N8R8 |
| Motor drivers | 2× TMC2209 SilentStepStick, 32× microstep |
| Motors | 2× NEMA17, 4.2kg-cm, 1.8°/step |
| GPS | NEO-M8N with ceramic active antenna |
| IMU | MPU9250 9-axis (I2C 0x68) |
| Encoders | 2× 1000PPR AB-phase NPN open collector |
| Display | 1.3" 128×64 OLED (I2C 0x3C) |
| Power | 12V DC adapter → LM2596 buck → 5V → ESP32 |
| PCB | Custom 2-layer, 150×100mm |

---

## GPIO Assignment

| GPIO | Function |
|---|---|
| GPIO1 | AZ STEP |
| GPIO2 | AZ DIR |
| GPIO3 | ALT STEP |
| GPIO4 | ALT DIR |
| GPIO5 | TMC AZ UART (PDN) |
| GPIO6 | TMC ALT UART (PDN) |
| GPIO7 | TMC EN (both, active LOW) |
| GPIO8 | GPS TX |
| GPIO9 | GPS RX |
| GPIO10 | I2C SDA (IMU + Display) |
| GPIO11 | I2C SCL (IMU + Display) |
| GPIO12 | ALT Encoder A |
| GPIO13 | ALT Encoder B |
| GPIO14 | AZ Encoder A |
| GPIO15 | AZ Encoder B |
| GPIO19 | D-PAD CENTER |
| GPIO20 | D-PAD UP |
| GPIO21 | D-PAD DOWN |
| GPIO35 | D-PAD LEFT |
| GPIO36 | D-PAD RIGHT |
| GPIO37 | Toggle switch |
| GPIO38 | LED PWR (green) |
| GPIO39 | LED GPS (blue) |
| GPIO40 | LED TRACK (yellow) |
| GPIO41 | Battery ADC (12V → 3V via 27K+10K divider) |

---

## Repository Structure

```
Actively-Stabilized-Dobsonian-Telescope/
│
├── README.md
├── BOM.csv                          ← Full bill of materials with links
│
├── PCB/
│   ├── nigga.kicad_pro              ← KiCad project file
│   ├── nigga.kicad_sch              ← Schematic
│   ├── nigga.kicad_pcb              ← PCB layout
│   ├── gerbers/                     ← Gerber + drill files (zip for JLCPCB)
│   │   └── pcb_com.zip
│   ├── pcb_com_bom.csv              ← PCB BOM from KiCad
│   ├── pcb_com_positions.csv        ← Component placement file
│   ├── pcb_com_designators.csv      ← Designator reference
│   └── netlist.ipc                  ← IPC netlist
│
├── CAD/
│   ├── telescope.step               ← Full 3D assembly (STEP)
│   └── onshape_link.txt             ← Link to OnShape public document
│
├── firmware/
│   └── (in progress)
│
└── docs/
    └── (wiring diagrams, references)
```

---

## Build Progress

- [x] Mirror selection and optics sourcing
- [x] Mechanical design (OnShape)
- [x] Drive system design (AZ ring gear + ALT belt)
- [x] Schematic design (KiCad)
- [x] PCB layout and fabrication files
- [x] 3D printed parts design
- [ ] Firmware (in progress)
- [ ] Physical assembly
- [ ] Testing and calibration

---

## 3D Printed Parts

All parts printed in PETG. Source files in OnShape (link in `CAD/onshape_link.txt`).

| Part | Infill | Layer height |
|---|---|---|
| AZ ring gear segments (×6) | 60% | 0.15mm |
| ALT GT2 pulley 120T | 60% | 0.15mm |
| AZ pinion 20T | 80% | 0.15mm |
| ALT pinion 20T | 80% | 0.15mm |
| Mirror cell | 40% | 0.2mm |
| Spider hub | 40% | 0.2mm |
| Secondary mirror holder | 40% | 0.2mm |
| Motor mounts (×2) | 40% | 0.2mm |
| Tube rings (×2) | 20% | 0.2mm |
| Focuser mounting plate | 40% | 0.2mm |

---

## PCB

Custom 2-layer PCB designed in KiCad. All major modules (ESP32, GPS, IMU, motor drivers) are socketed for easy replacement and upgrades.

**Key design decisions:**
- TMC2209 modules in sockets — swappable without desoldering
- ESP32-S3 DevKit in pin headers — removable for programming
- LM2596 buck converter module for 12V→5V
- Separate power rails for motors (12V) and logic (3.3V)
- Ground planes on both F.Cu and B.Cu layers
- 1.5mm traces for power, 0.5mm for signals

Gerber files ready for JLCPCB or PCBWay in `PCB/gerbers/pcb_com.zip`.

---

## Acknowledgements

- **Marcos ATM** — 2" Crayford focuser design: https://marcosatm.com/2020/06/04/2in-crayford-focuser/
- **Thingiverse** — Focuser STL files: https://www.thingiverse.com/thing:4424419
- **Tejraj & Co.** — GSO optics supplier, India: +91 98339 74700

---

## Some Pic 

<img width="1366" height="768" alt="Screenshot (112)" src="https://github.com/user-attachments/assets/771aa57c-9597-4973-8cef-a4f18cf81e2d" />
<img width="1366" height="768" alt="Screenshot (111)" src="https://github.com/user-attachments/assets/e3895957-6f75-447b-af4f-90dfd571d086" />
<img width="1366" height="768" alt="Screenshot (110)" src="https://github.com/user-attachments/assets/097795c6-3ed9-4f8c-820e-c3b51882628f" />
<img width="1366" height="768" alt="Screenshot (109)" src="https://github.com/user-attachments/assets/b61bcb87-119a-4d9f-b46a-b847cd0b9472" />
<img width="1366" height="768" alt="Screenshot (108)" src="https://github.com/user-attachments/assets/37ba76e6-86c3-4e19-8897-859fb1bafdf7" />
<img width="1366" height="768" alt="Screenshot (107)" src="https://github.com/user-attachments/assets/f5301744-2b80-4744-89ed-8ce57f52b618" />
<img width="1366" height="768" alt="Screenshot (106)" src="https://github.com/user-attachments/assets/8aa45c78-a365-4c10-8717-8f79d5b7ffc1" />
<img width="1366" height="768" alt="Screenshot (105)" src="https://github.com/user-attachments/assets/684029d0-83c8-4970-8f0b-8175fb858d5b" />
<img width="1366" height="768" alt="Screenshot (104)" src="https://github.com/user-attachments/assets/d704a59b-ce2a-4149-9fbc-89dd4fe5b1ce" />
<img width="1366" height="768" alt="Screenshot (103)" src="https://github.com/user-attachments/assets/b5d06aca-fe82-4922-8fce-56c20bce4997" />
<img width="1366" height="768" alt="Screenshot (102)" src="https://github.com/user-attachments/assets/45524340-e2dc-4e2f-b646-5d197583c3bc" />
<img width="1366" height="768" alt="Screenshot (101)" src="https://github.com/user-attachments/assets/5a4fd256-ea49-4c7d-8bc0-f5a90c2ec326" />
<img width="1366" height="768" alt="Screenshot (100)" src="https://github.com/user-attachments/assets/d4aecaa3-bce9-4b8d-b4de-8d11a063df9e" />
<img width="1366" height="768" alt="Screenshot (99)" src="https://github.com/user-attachments/assets/9860db77-5bf3-419b-9444-9a482b1de0bf" />

---

## Bill of Materials

| # | Component | Qty | Unit (₹) | Total (₹) | Source |
|---|---|---|---|---|---|
| 1 | [GSO 8" f/5 primary mirror](https://www.tejraj.com/product/gso-8-f5-parabolic-mirror) | 1 | ₹13,000 | ₹13,000 | Tejraj & Co. |
| 2 | [GSO 50mm secondary mirror](https://www.tejraj.com/product/50mm-secondary-mirror) | 1 | ₹5,500 | ₹5,500 | Tejraj & Co. |
| 3 | [32mm Plossl eyepiece](https://www.tejraj.com/product/32mm-plossl-eyepiece-125) | 1 | ₹3,350 | ₹3,350 | Tejraj & Co. |
| 4 | GSO 2" Crayford focuser + hardware | 1 | TBD | TBD | Local / Tejraj |
| 5 | [ESP32-S3-DevKitC-1-N8R8](https://www.electropi.in/espressif-esp32-s3-devkitc-1-n8r8-development-board) | 1 | ₹1,500 | ₹1,500 | Electropi.in |
| 6 | [NEMA17 4.2kg-cm stepper motor](https://robu.in/product/nema17-4-2-kgcm-stepper-motor/) | 2 | ₹700 | ₹1,400 | Robu.in |
| 7 | [TMC2209 stepper driver module](https://www.electropi.in/tmc2209-stepper-motor-driver-module) | 2 | ₹400 | ₹800 | Electropi.in |
| 8 | [1000PPR optical encoder (AB phase NPN)](https://www.amazon.in/Techtonics-2-Phase-Incremental-Optical-Quadrature/dp/B0DCK8HMHN) | 2 | ₹1,850 | ₹3,700 | Amazon.in |
| 9 | [LM2596 buck converter module](https://robu.in/product/lm2596s-dc-dc-buck-converter-power-supply/) | 1 | ₹45 | ₹45 | Robu.in |
| 10 | [MPU9250 9-axis IMU](https://www.electropi.in/mpu9250-9-axis-gyro-accelerometer-module) | 1 | ₹350 | ₹350 | Electropi.in |
| 11 | [NEO-M8N GPS module](https://www.electropi.in/neo-m8n-gps-module-with-ceramic-active-antenna) | 1 | ₹900 | ₹900 | Electropi.in |
| 12 | [1.3" 128×64 OLED display](https://www.electropi.in/1.3-inch-128x64-oled-display-screen-module-with-spi-serial-interface-v2) | 1 | ₹350 | ₹350 | Electropi.in |
| 13 | Custom PCB (2-layer, 150×100mm) | 1 | ₹2,500 | ₹2,500 | JLCPCB / PCBWay |
| 14 | Resistor 4.7K (encoder pullup) | 4 | TBD | TBD | Local Palasia |
| 15 | Resistor 330Ω (LED) | 3 | TBD | TBD | Local Palasia |
| 16 | Resistor 10K (EN pullup + I2C) | 3 | TBD | TBD | Local Palasia |
| 17 | Resistor 27K (ADC divider) | 1 | TBD | TBD | Local Palasia |
| 18 | Capacitor 100µF electrolytic | 2 | TBD | TBD | Local Palasia |
| 19 | LED green + blue + yellow 5mm | 3 | TBD | TBD | Local Palasia |
| 20 | JST 4-pin connector (motor ×2, display ×1, GPS ×1) | 4 | TBD | TBD | Local Palasia |
| 21 | JST 5-pin connector (encoder) | 2 | TBD | TBD | Local Palasia |
| 22 | JST 10-pin connector (IMU) | 1 | TBD | TBD | Local Palasia |
| 23 | Toggle switch SPDT | 2 | TBD | TBD | Local Palasia |
| 24 | 5-way tactile switch breakout 7-pin | 1 | TBD | TBD | Robu.in / Amazon |
| 25 | Pin headers male/female | — | TBD | TBD | Local Palasia |
| 26 | 12V DC adapter 3A | 1 | TBD | TBD | Local / Amazon |
| 27 | Wiring (various) | — | TBD | TBD | Local Palasia |
| 28 | Aluminium tube (telescope body) | 1 | TBD | TBD | Local Indore |
| 29 | 12mm plywood (rocker box + ground board) | 1 | ₹1,000 | ₹1,000 | Local Indore |
| 30 | Plywood laser cutting | 1 | ₹1,000 | ₹1,000 | Local Indore |
| 31 | [12" lazy susan bearing](https://www.amazon.in/BOSCO-Swivel-Turntable-Rotating-Furniture/dp/B0GSGZFKGD) | 1 | ₹650 | ₹650 | Amazon.in |
| 32 | Spider vane steel strip 0.8×15×109mm | 4 | TBD | TBD | Local Palasia |
| 33 | 25mm aluminium pipe (ALT axis) | 1 | TBD | TBD | Local Indore |
| 34 | 6205-2RS bearing 25mm bore | 2 | TBD | TBD | Local Indore |
| 35 | GT2 belt 6mm | 1 | TBD | TBD | Robu.in |
| 36 | Teflon pads | 3 | TBD | TBD | Local hardware |
| 37 | M3 screws (secondary ×3, tube ×4, pulleys ×4) | 11 | TBD | TBD | Local Palasia |
| 38 | M4 screws (big pulley ×2) | 2 | TBD | TBD | Local Palasia |
| 39 | M5 screws (primary cell ×6) | 6 | TBD | TBD | Local Palasia |
| 40 | M6 screws (tube rings ×6) | 6 | TBD | TBD | Local Palasia |
| 41 | Nylon M2 screws (mirror holder) | 6 | TBD | TBD | Local Palasia |
| 42 | All PETG 3D printed parts | 1 | ₹10,000 | ₹10,000 | 3Ding.in / Aprintpro |
| 43 | RTV silicone (mirror mounting) | 1 | TBD | TBD | Local hardware |
| — | **Total (known)** | | | **₹46,045** | **≈ $490 USD** |

---

*Built in Indore, Madhya Pradesh, India.*
