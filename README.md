# BIGBOY-ADV
A custom Game Boy inspired Console for running retrogames using emulationstation on Retropie , using a Raspberry Pi 3B+ 

# BIGBOY-ADV

<p align="center">
  <img src="docs/images/BMO-FRONT.png" width="47%" alt="BIGBOY-ADV Front CAD Assembly">
  <img src="docs/images/BMO-BACK.png" width="47%" alt="BIGBOY-ADV Rear CAD Assembly">
</p>

<h3 align="center">
A Custom Battery-Powered Embedded Linux Handheld Console
</h3>

<p align="center">
Raspberry Pi 3B+ • Embedded Linux • RetroPie • GPIONext • fbcp-ili9341 • Power Electronics • Analog Audio • CAD • 3D Printing
</p>

---

## Project Overview

**BIGBOY-ADV** is a custom portable embedded Linux gaming console designed and built around the Raspberry Pi 3B+.

Although the project retained the internal development name **BMO**, the final enclosure and user interface were designed as a custom **white Pokémon-inspired handheld console**.

The project began as a relatively simple attempt to build a portable Game Boy Advance emulator.

It eventually evolved into a complete embedded system requiring the integration of:

- Linux system configuration
- Raspberry Pi hardware
- GPIO-based physical controls
- GPIONext Linux input handling
- 3.5-inch SPI TFT display
- `fbcp-ili9341` framebuffer acceleration
- SPI bus optimization
- Raspberry Pi performance tuning
- dual-rail power distribution
- 2S LiPo battery architecture
- battery protection
- integrated USB Type-C charging
- DC-DC voltage regulation
- analog audio amplification
- dedicated audio PCB
- hardware volume control
- AUX/internal-speaker selection
- audio filtering and EMI reduction
- thermal management
- heatsinks and enclosure airflow
- custom boot splash screens
- customized EmulationStation UI
- automated boot services
- game metadata and artwork scraping
- custom CAD
- iterative 3D-printed enclosure development

The final result is a self-contained handheld console capable of running a library of more than **400 Game Boy Advance titles**.

The goal of this repository is not only to showcase the finished system.

It documents the complete engineering process:

> **Requirements → Architecture → Prototyping → Failure → Debugging → Optimization → Integration → Validation**

This repository is also intended to serve as a reproducible reference for anyone interested in building a similar Raspberry Pi-based handheld console.

---

# Table of Contents

1. [Final System](#1-final-system)
2. [Project Objectives](#2-project-objectives)
3. [System Specifications](#3-system-specifications)
4. [System Architecture](#4-system-architecture)
5. [Repository Structure](#5-repository-structure)
6. [Bill of Materials](#6-bill-of-materials)
7. [Tools Required](#7-tools-required)
8. [Mechanical Design](#8-mechanical-design)
9. [Power-System Architecture](#9-power-system-architecture)
10. [Why I Used a 2S Battery Architecture](#10-why-i-used-a-2s-battery-architecture)
11. [Battery Protection and USB-C Charging](#11-battery-protection-and-usb-c-charging)
12. [DC-DC Regulation](#12-dc-dc-regulation)
13. [Dual Power Rails](#13-dual-power-rails)
14. [Power Budget](#14-power-budget)
15. [Power-System Assembly Guide](#15-power-system-assembly-guide)
16. [GPIO Controller Hardware](#16-gpio-controller-hardware)
17. [Backlit XYAB Buttons](#17-backlit-xyab-buttons)
18. [GPIO Pin Mapping](#18-gpio-pin-mapping)
19. [Installing GPIONext](#19-installing-gpionext)
20. [Configuring GPIONext at Boot](#20-configuring-gpionext-at-boot)
21. [SPI Display Integration](#21-spi-display-integration)
22. [Why the Original GoodTFT Driver Was Replaced](#22-why-the-original-goodtft-driver-was-replaced)
23. [Installing fbcp-ili9341](#23-installing-fbcp-ili9341)
24. [SPI Display Optimization](#24-spi-display-optimization)
25. [Raspberry Pi Performance Tuning](#25-raspberry-pi-performance-tuning)
26. [Thermal Management](#26-thermal-management)
27. [Audio-System Architecture](#27-audio-system-architecture)
28. [Dedicated Audio PCB](#28-dedicated-audio-pcb)
29. [LM386 Filtering and EMI Reduction](#29-lm386-filtering-and-emi-reduction)
30. [Volume and Audio Output Controls](#30-volume-and-audio-output-controls)
31. [Installing RetroPie](#31-installing-retropie)
32. [Customizing EmulationStation](#32-customizing-emulationstation)
33. [Game Metadata and Artwork](#33-game-metadata-and-artwork)
34. [Custom Splash Screens](#34-custom-splash-screens)
35. [Boot Automation](#35-boot-automation)
36. [System Integration](#36-system-integration)
37. [Bring-Up Procedure](#37-bring-up-procedure)
38. [Engineering Challenges and Solutions](#38-engineering-challenges-and-solutions)
39. [Testing and Validation](#39-testing-and-validation)
40. [Performance Results](#40-performance-results)
41. [Known Limitations](#41-known-limitations)
42. [Future Improvements](#42-future-improvements)
43. [Resources and Credits](#43-resources-and-credits)
44. [Project Status](#44-project-status)

---

# 1. Final System

> Add photographs of the completed console here.

```text
docs/images/final/front.jpg
docs/images/final/back.jpg
docs/images/final/side.jpg
docs/images/final/running-game.jpg
```

The completed BIGBOY-ADV is a portable embedded Linux handheld console integrating all compute, control, display, audio, power, charging, and user-interface hardware inside a custom 3D-printed enclosure.

### Core Features

- Raspberry Pi 3B+
- 3.5-inch SPI TFT
- `fbcp-ili9341` accelerated framebuffer driver
- Raspberry Pi performance tuning
- Direct GPIO gaming controls
- GPIONext Linux input interface
- D-pad
- XYAB controls
- LT and RT shoulder controls
- Start and Select
- Master/system controls
- Backlit XYAB tactile switches
- Dedicated backlight switch
- 2S LiPo battery system
- 2S BMS
- 2S 1 A charging module
- USB Type-C charging interface
- Buck-regulated 5 V supply
- Separate high-power and low-power distribution rails
- Dedicated LM386 audio PCB
- 100 µF filtering around the LM386 power stage
- Additional audio filtering and EMI mitigation
- 0.8 W internal speaker
- Hardware volume control
- 3.5 mm AUX output
- Hardware AUX/internal-speaker selection
- Amplifier power isolation when not required
- Custom splash screens
- Handheld-optimized frontend layout
- Scraped game metadata and artwork
- Automated RetroPie and GPIONext startup
- Passive thermal management
- Raspberry Pi and buck-converter heatsinks
- Enclosure ventilation
- Custom CAD and 3D-printed enclosure
- 400+ GBA titles

---

# 2. Project Objectives

The primary objective was to convert a Raspberry Pi development board into a complete portable embedded product.

The design requirements were:

1. Portable battery-powered operation.
2. Stable 5 V delivery during processor and display load transients.
3. Direct GPIO controls without an external USB gamepad controller.
4. A complete Linux input interface.
5. Acceptable gaming performance on an SPI-connected display.
6. Integrated speaker audio and external AUX output.
7. User-adjustable hardware volume.
8. Switchable button illumination.
9. Integrated battery charging.
10. Thermal management suitable for extended gaming sessions.
11. Automated boot into the gaming frontend.
12. A custom user interface designed for the 3.5-inch screen.
13. A serviceable 3D-printed enclosure.
14. Reproducible documentation.

---

# 3. System Specifications

| Parameter | Implementation |
|---|---|
| Main Computer | Raspberry Pi 3B+ |
| Operating System | Raspberry Pi OS / RetroPie |
| Frontend | EmulationStation |
| Primary Platform | Game Boy Advance |
| Game Library | 400+ titles |
| Display | 3.5-inch SPI TFT |
| Display Driver | fbcp-ili9341 |
| Controls | Direct Raspberry Pi GPIO |
| Linux Input Driver | GPIONext |
| Control Layout | D-Pad, XYAB, LT, RT, Start, Select, System Controls |
| XYAB Switches | 12 × 12 × 7.3 mm illuminated tactile switches |
| Battery Architecture | 2S LiPo |
| Cell Rating | 3.7 V, 3600 mAh each |
| Pack Nominal Voltage | 7.4 V |
| Pack Maximum Voltage | 8.4 V |
| Pack Capacity | 3600 mAh |
| Nominal Stored Energy | Approximately 26.6 Wh |
| Battery Protection | 2S BMS |
| Charging | 2S, 1 A charger |
| Charging Interface | USB Type-C |
| Main Conversion | 7.4 V nominal battery bus → regulated 5 V |
| Power Distribution | Separate high-power and low-power rails |
| Audio Amplifier | LM386 |
| Speaker | 0.8 W |
| Main Audio Filter Capacitor | 100 µF |
| Volume Control | Hardware potentiometer |
| External Audio | 3.5 mm AUX |
| Thermal Management | Enclosure airflow + heatsinks |
| Mechanical Design | Custom CAD and 3D printing |
| Final Theme | White Pokémon-inspired handheld |

---

# 4. System Architecture

```text
                               2S LiPo Pack
                            7.4 V Nominal Bus
                                   │
                                   ▼
                                2S BMS
                                   │
                  ┌────────────────┴────────────────┐
                  │                                 │
                  ▼                                 ▼
          USB-C Charging Path                System Power Path
          2S / 1 A Charger                          │
                                                    ▼
                                             Master Power
                                                    │
                                                    ▼
                                               5 V Buck
                                                    │
                              ┌─────────────────────┴─────────────────────┐
                              │                                           │
                              ▼                                           ▼
                       HIGH-POWER RAIL                             LOW-POWER RAIL
                              │                                           │
                    ┌─────────┼─────────┐                      ┌──────────┼─────────┐
                    │         │         │                      │          │         │
                    ▼         ▼         ▼                      ▼          ▼         ▼
              Raspberry Pi   TFT     Main Loads             GPIO       LEDs    Low-Power
                   3B+      Display                         Inputs              Peripherals
                    │
        ┌───────────┼───────────────────────┐
        │           │                       │
        ▼           ▼                       ▼
   GPIONext     fbcp-ili9341            Audio Output
        │           │                       │
        ▼           ▼                       ▼
 Linux Input    SPI Display             Audio Filter
   Device       Pipeline                    │
        │                                   ▼
        ▼                             Volume Control
  RetroPie /                                 │
EmulationStation                             ▼
                                        LM386 PCB
                                             │
                                ┌────────────┴────────────┐
                                │                         │
                                ▼                         ▼
                          0.8 W Speaker               AUX Output
```

---

# 5. Repository Structure

```text
BIGBOY-ADV/
│
├── README.md
├── LICENSE
│
├── cad/
│   ├── solidworks/
│   ├── step/
│   ├── stl/
│   ├── drawings/
│   └── renders/
│
├── hardware/
│   ├── bom/
│   │   └── BOM.csv
│   ├── gpio/
│   │   ├── gpio-pinout.md
│   │   └── wiring-diagram.png
│   ├── power/
│   │   ├── power-architecture.png
│   │   ├── power-budget.xlsx
│   │   └── wiring-diagram.png
│   └── audio/
│       ├── schematic/
│       ├── pcb/
│       ├── gerbers/
│       └── assembly/
│
├── config/
│   ├── boot/
│   │   └── config.txt
│   ├── fbcp-ili9341/
│   │   ├── build-command.txt
│   │   └── service/
│   ├── gpionext/
│   │   ├── config/
│   │   └── service/
│   ├── retropie/
│   └── emulationstation/
│       └── theme/
│
├── scripts/
│   ├── install/
│   ├── startup/
│   └── backup/
│
├── assets/
│   ├── splashscreens/
│   └── branding/
│
├── docs/
│   ├── build-log/
│   ├── troubleshooting/
│   ├── images/
│   │   ├── cad/
│   │   ├── hardware/
│   │   ├── assembly/
│   │   ├── software/
│   │   ├── testing/
│   │   └── final/
│   └── videos/
│
└── tests/
    ├── power/
    ├── thermal/
    ├── display/
    ├── controls/
    └── runtime/
```

---

# 6. Bill of Materials

The following BOM contains the known major components.

Add exact manufacturers, module models, quantities, suppliers, and prices as the build documentation is completed.

| Category | Component | Specification | Qty | Manufacturer / Model | Cost | Notes |
|---|---|---:|---:|---|---:|---|
| Compute | Raspberry Pi 3B+ | SBC | 1 | Raspberry Pi 3B+ | ___ | Main computer |
| Display | SPI TFT | 3.5-inch | 1 | ___ | ___ | Add controller IC |
| Battery | LiPo Cell | 3.7 V, 3600 mAh | 2 | ___ | ___ | 2S pack |
| Protection | BMS | 2S | 1 | ___ | ___ | Add current rating |
| Charging | Charger | 2S, 1 A, USB-C | 1 | ___ | ___ | Integrated charging |
| Regulation | Buck Converter | 7.4 V nominal → 5 V | 1 | ___ | ___ | Add rated current |
| Audio | LM386 | Audio amplifier | 1 | ___ | ___ | Dedicated audio PCB |
| Audio | Speaker | 0.8 W | 1 | ___ | ___ | Internal speaker |
| Audio | Potentiometer | ___ kΩ | 1 | ___ | ___ | Volume control |
| Audio | AUX Jack | 3.5 mm | 1 | ___ | ___ | External audio |
| Audio | Capacitor | 100 µF | ___ | ___ | ___ | LM386 filtering |
| Controls | Illuminated Tactile Switch | 12×12×7.3 mm | 4 | ___ | ___ | XYAB |
| Controls | Tactile Switch | ___ | ___ | ___ | ___ | D-pad/system controls |
| Controls | Shoulder Switch | ___ | 2 | ___ | ___ | LT / RT |
| Controls | LED Resistor | ___ Ω | 4 | ___ | ___ | Individual XYAB resistors |
| Switching | Backlight Switch | ___ | 1 | ___ | ___ | LED control |
| Switching | Audio Selector | ___ | 1 | ___ | ___ | AUX/speaker selection |
| Power | Metal Ring Button | 12 mm | 1 | ___ | ___ | Master power control |
| Thermal | Raspberry Pi Heatsink | ___ | ___ | ___ | ___ | Passive cooling |
| Thermal | Buck Heatsink | ___ | ___ | ___ | ___ | Converter cooling |
| Mechanical | Enclosure Filament | ___ | ___ | ___ | ___ | Add material |
| Hardware | Fasteners | ___ | ___ | ___ | ___ | Enclosure assembly |
| Wiring | Wire | ___ AWG | ___ | ___ | ___ | Power/signal wiring |
| Misc. | Connectors | ___ | ___ | ___ | ___ | Internal connections |

---

# 7. Tools Required

## Hardware Tools

- Soldering iron
- Solder
- Flux
- Multimeter
- Wire cutters
- Wire strippers
- Heat-shrink tubing
- Small screwdrivers
- USB power meter
- Bench power supply recommended
- Oscilloscope recommended for power/audio debugging
- Logic analyzer optional

## Mechanical Tools

- 3D printer
- CAD software
- Calipers
- Files and deburring tools
- Drill bits if post-processing is required

## Software Tools

- Raspberry Pi Imager
- SSH client
- Git
- CMake
- GCC/G++
- RetroPie Setup Script
- Linux command-line tools

---

# 8. Mechanical Design

## 8.1 Design Direction

The final console was designed as a **white Pokémon-inspired handheld**, while retaining the internal project name BIGBOY-ADV/BMO.

The enclosure was designed from scratch in SolidWorks.

The front assembly contains:

- 3.5-inch display
- D-pad
- illuminated XYAB controls
- Start and Select controls
- system/master control
- front speaker opening
- Pokémon-inspired lightning-bolt detail

The enclosure also provides:

- side-mounted controls
- charging and audio interfaces
- ventilation openings
- rear service panel
- internal battery volume
- Raspberry Pi mounting
- power-electronics mounting
- audio-PCB mounting
- wire-routing space

> Add front CAD render.

> Add rear CAD render.

> Add exploded assembly view.

> Add cross-section showing component placement.

## 8.2 Design Constraints

The mechanical design had to account for:

- display viewing area
- switch travel
- button clearances
- D-pad movement
- PCB mounting
- battery clearance
- heatsink height
- airflow
- speaker volume
- wiring bend radius
- connector accessibility
- charging access
- enclosure wall thickness
- print orientation
- assembly sequence
- maintenance

## 8.3 Iterative Development

The enclosure was revised as the electronics evolved.

Important mechanical changes included:

- additional airflow openings
- improved internal clearances
- heatsink accommodation
- dedicated audio-PCB space
- revised wire routing
- rear service access
- improved external interface placement

This was a hardware-aware CAD process.

The enclosure and electronics were developed together rather than as independent subsystems.

---

# 9. Power-System Architecture

The final power system uses a 2S LiPo battery pack.

```text
USB TYPE-C INPUT
       │
       ▼
2S / 1 A CHARGER
       │
       ▼
     2S BMS
       │
       ▼
2S LiPo Battery Pack
       │
       ▼
Master Power Control
       │
       ▼
  5 V Buck Converter
       │
       ▼
Power Distribution
       │
       ├──────────── HIGH-POWER RAIL
       │
       └──────────── LOW-POWER RAIL
```

---

# 10. Why I Used a 2S Battery Architecture

An earlier approach relied on stepping a lower battery voltage upward using a boost converter.

This proved unreliable.

The Raspberry Pi, display, and system peripherals produced rapid current-demand changes.

The boost-converter implementation experienced difficulty maintaining the required output voltage during these load transients.

The result was unstable system operation.

The power architecture was redesigned around a 2S battery pack.

The new architecture uses:

```text
7.4 V NOMINAL BATTERY BUS

          │

          ▼

     BUCK CONVERTER

          │

          ▼

     REGULATED 5 V
```

This provided additional voltage headroom and allowed the system voltage to be regulated downward.

The change from boost conversion to a 2S buck-regulated architecture was one of the most important electrical improvements made during the project.

### Engineering Lesson

A power converter should not be selected only from the average current consumption of a system.

Transient load response, wiring losses, converter efficiency, thermal performance, and output capacitance can determine whether a computing platform remains stable during real workloads.

---

# 11. Battery Protection and USB-C Charging

The console uses:

- two 3.7 V, 3600 mAh LiPo cells in series
- 2S BMS
- 2S, 1 A charging module
- USB Type-C charging input

The battery pack specifications are:

```text
Nominal Voltage:       7.4 V

Maximum Voltage:       8.4 V

Capacity:               3600 mAh

Nominal Stored Energy:  26.64 Wh
```

The charging system allows the cells to remain installed inside the enclosure.

## Important Safety Note

Only use LiPo cells, charging modules, BMS boards, wiring, and connectors rated for the intended voltage and current.

Verify the charger topology and BMS wiring against the specific modules being used.

Do not assume that every board sold as a "2S BMS" or "2S charger" uses the same terminals or supports simultaneous charge-and-load operation.

---

# 12. DC-DC Regulation

The battery voltage is reduced to the 5 V system rail using a buck converter.

The regulator must support:

- Raspberry Pi load transients
- display load
- audio load
- GPIO peripherals
- backlighting
- conversion losses
- thermal derating
- sufficient transient headroom

## Final Converter

| Parameter | Value |
|---|---|
| Converter Model | ___ |
| Input Range | ___ |
| Output Voltage | 5.0 V |
| Rated Output Current | ___ |
| Measured Continuous Current | ___ |
| Peak Current Tested | ___ |
| Switching Frequency | ___ |
| Measured Efficiency | ___ |
| Heatsink | Yes |
| Maximum Measured Temperature | ___ °C |

---

# 13. Dual Power Rails

The regulated system supply is distributed through separate high-power and low-power rails.

## High-Power Rail

Supplies:

- Raspberry Pi 3B+
- display
- main system loads

## Low-Power Rail

Supplies:

- GPIO-related circuits
- button LEDs
- low-power peripheral electronics

The design improved power-distribution organization and simplified wiring and debugging.

> Add actual power-distribution schematic here.

---

# 14. Power Budget

Complete this table using measured current values.

Measurements should ideally be taken at the battery input and regulated 5 V output.

| Operating State | Voltage | Current | Power | Notes |
|---|---:|---:|---:|---|
| System Off | ___ V | ___ A | ___ W | Quiescent draw |
| Boot | ___ V | ___ A | ___ W | Peak during Linux boot |
| EmulationStation Idle | ___ V | ___ A | ___ W | Backlight on |
| EmulationStation Idle | ___ V | ___ A | ___ W | Backlight off |
| GBA Gameplay | ___ V | ___ A | ___ W | Typical game |
| Maximum Observed Load | ___ V | ___ A | ___ W | Stress condition |
| Internal Speaker Active | ___ V | ___ A | ___ W | Typical volume |
| AUX Mode | ___ V | ___ A | ___ W | Amplifier disabled |
| Charging | ___ V | ___ A | ___ W | System off |
| Charging + Operating | ___ V | ___ A | ___ W | If supported |

## Estimated Runtime

```text
Battery Energy = 26.64 Wh

Measured Average System Power = ______ W

Estimated Ideal Runtime = 26.64 / ______ = ______ hours

Measured Runtime = ______ hours
```

---

# 15. Power-System Assembly Guide

## Step 1 — Validate the Battery Cells

Before assembly:

1. Verify both cells are the same chemistry.
2. Verify compatible capacities.
3. Measure individual cell voltages.
4. Confirm the cells are at similar states of charge before series connection.
5. Inspect for physical damage.

## Step 2 — Wire the 2S BMS

Follow the pinout for the specific BMS.

Typical labels may include:

```text
B-
B1 / BM
B+
P-
P+
```

Do not copy a generic wiring diagram without checking the board documentation.

## Step 3 — Connect the Charger

Verify:

- charger output voltage
- maximum charge current
- USB-C input implementation
- charge termination behavior

## Step 4 — Configure the Buck Converter

Before connecting the Raspberry Pi:

1. Connect the battery system.
2. Power the converter.
3. Measure the output voltage.
4. Adjust the output to 5.0 V.
5. Test with a dummy load.
6. Observe voltage droop.
7. Check converter temperature.

## Step 5 — Validate the Rails

Test each rail independently.

```text
BATTERY → BMS → SWITCH → BUCK → POWER RAILS
```

Only connect the Raspberry Pi after stable voltage has been verified.

---

# 16. GPIO Controller Hardware

The console uses direct GPIO controls instead of a USB controller PCB.

The physical control interface includes:

- D-pad
- X
- Y
- A
- B
- LT
- RT
- Start
- Select
- system controls

The design reduces hardware overhead and integrates the physical controls directly into the Linux input subsystem.

---

# 17. Backlit XYAB Buttons

The XYAB controls use:

```text
12 × 12 × 7.3 mm
Illuminated Tactile Switches
```

Each LED uses its own current-limiting resistor.

```text
LED X ─── RX ─── SUPPLY

LED Y ─── RY ─── SUPPLY

LED A ─── RA ─── SUPPLY

LED B ─── RB ─── SUPPLY
```

Individual resistors were used because a shared current-limiting resistor produced inconsistent LED brightness.

The final design improved:

- brightness uniformity
- predictable LED current
- reliability
- visual appearance

The backlight supply can also be disabled using a dedicated physical switch.

---

# 18. GPIO Pin Mapping

Replace the blanks with the final BCM GPIO assignments.

| Control | BCM GPIO | Physical Pin | Pull Configuration | GPIONext Mapping |
|---|---:|---:|---|---|
| D-Pad Up | ___ | ___ | ___ | KEY_UP |
| D-Pad Down | ___ | ___ | ___ | KEY_DOWN |
| D-Pad Left | ___ | ___ | ___ | KEY_LEFT |
| D-Pad Right | ___ | ___ | ___ | KEY_RIGHT |
| A | ___ | ___ | ___ | ___ |
| B | ___ | ___ | ___ | ___ |
| X | ___ | ___ | ___ | ___ |
| Y | ___ | ___ | ___ | ___ |
| LT | ___ | ___ | ___ | ___ |
| RT | ___ | ___ | ___ | ___ |
| Start | ___ | ___ | ___ | ___ |
| Select | ___ | ___ | ___ | ___ |
| System Button 1 | ___ | ___ | ___ | ___ |
| System Button 2 | ___ | ___ | ___ | ___ |

---

# 19. Installing GPIONext

BIGBOY-ADV uses GPIONext to expose the physical GPIO buttons as Linux input devices.

Repository:

https://github.com/mholgatem/GPIOnext

## Clone the Repository

```bash
cd ~
git clone https://github.com/mholgatem/GPIOnext.git
cd GPIOnext
```

## Review the Project Documentation

Before installation, check the upstream README for:

- supported operating-system versions
- dependencies
- configuration format
- installation instructions

## Install and Configure

Use the installation procedure documented by the upstream project.

After installation, verify that the virtual input device is visible.

```bash
cat /proc/bus/input/devices
```

Install `evtest` if required:

```bash
sudo apt update
sudo apt install evtest
```

Test the controller:

```bash
sudo evtest
```

Press each physical control and confirm that:

- only one event is generated per press
- the correct key is reported
- no GPIO remains stuck
- there are no repeated false events

---

# 20. Configuring GPIONext at Boot

The final console starts the GPIO input service automatically.

First verify the exact service name created by your installation.

```bash
systemctl list-unit-files | grep -i gpio
```

Then inspect the service.

```bash
systemctl status <gpionext-service-name>
```

Enable it at boot if it is not already enabled:

```bash
sudo systemctl enable <gpionext-service-name>
sudo systemctl start <gpionext-service-name>
```

Verify after reboot:

```bash
systemctl is-active <gpionext-service-name>
```

Store the final configuration in:

```text
config/gpionext/
```

---

# 21. SPI Display Integration

The console uses a 3.5-inch SPI TFT.

The initial display implementation functioned correctly for basic Linux output but performed poorly during gaming.

Observed problems included:

- screen tearing
- low effective frame rate
- slow framebuffer updates
- visible latency
- poor gaming experience

The display stack became a major optimization task.

---

# 22. Why the Original GoodTFT Driver Was Replaced

The initial GoodTFT/LCD-show-style display configuration was not fast enough for the intended gaming workload.

The SPI display was functional, but framebuffer transfer performance was the limiting factor.

The driver stack was replaced with `fbcp-ili9341`.

The resulting pipeline was:

```text
GPU FRAMEBUFFER

       │
       ▼

fbcp-ili9341

       │
       ▼

OPTIMIZED FRAMEBUFFER COPY

       │
       ▼

SPI TRANSFER

       │
       ▼

3.5-INCH TFT
```

This significantly improved display responsiveness and reduced the limitations of the original configuration.

---

# 23. Installing fbcp-ili9341

BIGBOY-ADV uses `fbcp-ili9341`.

Repository:

https://github.com/juj/fbcp-ili9341

## Install Build Dependencies

```bash
sudo apt update
sudo apt install git cmake build-essential
```

## Clone the Repository

```bash
cd ~
git clone https://github.com/juj/fbcp-ili9341.git
cd fbcp-ili9341
```

## Create a Build Directory

```bash
mkdir build
cd build
```

## Identify the Display Controller

Before compiling the driver, identify the actual LCD controller IC.

Examples supported by the project may include:

```text
ILI9341
ILI9486
ILI9488
ST7735
ST7789
HX8357D
```

Do not assume that every 3.5-inch display uses the same controller.

## Configure the Build

The actual BIGBOY-ADV CMake configuration should be stored in:

```text
config/fbcp-ili9341/build-command.txt
```

Template:

```bash
cmake \
  -D<DISPLAY_CONTROLLER>=ON \
  -DGPIO_TFT_DATA_CONTROL=<GPIO> \
  -DGPIO_TFT_RESET_PIN=<GPIO> \
  -DSPI_BUS_CLOCK_DIVISOR=<VALUE> \
  ..
```

## Compile

```bash
make -j$(nproc)
```

## Test Manually

```bash
sudo ./fbcp-ili9341
```

Do not configure automatic startup until the display works reliably during manual testing.

---

# 24. SPI Display Optimization

The display was optimized by tuning the SPI transfer rate.

The engineering tradeoff was:

```text
LOW SPI CLOCK
     │
     ├── Stable
     └── Slow Refresh

INCREASE SPI CLOCK
     │
     ├── Higher Throughput
     ├── Lower Latency
     └── Improved Gaming Experience

EXCESSIVE SPI CLOCK
     │
     ├── Display Corruption
     ├── Unstable Transfers
     ├── Signal Integrity Problems
     └── Reduced Reliability
```

## Final Display Configuration

| Parameter | Value |
|---|---|
| Display Controller | ___ |
| Resolution | ___ × ___ |
| SPI Bus | ___ |
| SPI Clock / Divider | ___ |
| Rotation | ___ |
| DMA Enabled | ___ |
| Scaling Mode | ___ |
| Stable Test Duration | ___ hours |
| Screen Tearing | ___ |
| Maximum Display Temperature | ___ °C |

---

# 25. Raspberry Pi Performance Tuning

The Raspberry Pi 3B+ was tuned to improve emulator and framebuffer performance.

The tuning process considered:

- CPU frequency
- GPU/core frequency
- voltage settings
- SPI framebuffer load
- emulator load
- temperature
- power consumption
- system stability

Store the final tested configuration in:

```text
config/boot/config.txt
```

## Final Stable Overclock Configuration

```ini
# BIGBOY-ADV Raspberry Pi 3B+ Configuration

arm_freq=____

core_freq=____

gpu_freq=____

sdram_freq=____

over_voltage=____
```

Only include values actually tested on the final console.

## Validation Procedure

After changing clock settings:

```bash
vcgencmd measure_temp
vcgencmd measure_clock arm
vcgencmd measure_volts
vcgencmd get_throttled
```

Run:

- repeated emulator sessions
- display stress tests
- thermal soak tests
- cold boots
- repeated reboots

Document any throttling or instability.

---

# 26. Thermal Management

Increasing display throughput and Raspberry Pi performance increased thermal load.

The final console includes:

- enclosure airflow openings
- passive ventilation
- heatsinks on the Raspberry Pi
- heatsink on the buck converter
- internal layout designed to avoid completely enclosing heat-generating components

> Add image showing ventilation openings.

> Add image showing Raspberry Pi heatsinks.

> Add image showing buck-converter heatsink.

## Thermal Test Table

| Test Condition | Ambient | Pi Idle | Gameplay | Maximum | Throttling |
|---|---:|---:|---:|---:|---|
| Stock Clock | ___ °C | ___ °C | ___ °C | ___ °C | ___ |
| Final Clock | ___ °C | ___ °C | ___ °C | ___ °C | ___ |
| 30 min Gameplay | ___ °C | ___ °C | ___ °C | ___ °C | ___ |
| 60 min Gameplay | ___ °C | ___ °C | ___ °C | ___ °C | ___ |

---

# 27. Audio-System Architecture

The console includes a dedicated analog audio subsystem.

```text
RASPBERRY PI AUDIO OUTPUT

            │
            ▼

       INPUT FILTERING

            │
            ▼

      VOLUME CONTROL

            │
            ▼

      LM386 AUDIO PCB

            │
            ▼

      OUTPUT FILTERING

            │
       ┌────┴────┐
       │         │
       ▼         ▼

  0.8 W SPEAKER  AUX OUTPUT
```

---

# 28. Dedicated Audio PCB

The audio subsystem was moved to a dedicated PCB.

The PCB integrates:

- LM386 amplifier
- volume-control interface
- filtering components
- power connections
- audio input
- speaker output
- associated passive components

This improved the project compared with a loose prototype-module implementation.

Benefits included:

- cleaner wiring
- repeatable assembly
- improved mechanical integration
- easier troubleshooting
- better audio signal routing
- reduced wiring length
- improved system serviceability

Add the following files:

```text
hardware/audio/schematic/
hardware/audio/pcb/
hardware/audio/gerbers/
hardware/audio/assembly/
```

> Add PCB schematic.

> Add PCB layout.

> Add fabricated PCB photograph.

> Add assembled PCB photograph.

---

# 29. LM386 Filtering and EMI Reduction

The audio system initially experienced unwanted noise.

Potential noise sources included:

- Raspberry Pi digital switching
- SPI bus activity
- buck-converter switching
- common power paths
- long wiring
- grounding
- amplifier gain
- high-current load transients

The final audio implementation uses additional filtering and improved routing.

A **100 µF capacitor** was used around the LM386 power stage as part of the supply filtering.

Document the final circuit here.

| Component | Value | Function |
|---|---:|---|
| C1 | ___ | Input coupling |
| C2 | ___ | Bypass |
| C3 | 100 µF | Supply filtering |
| C4 | ___ | Output coupling |
| C5 | ___ | High-frequency decoupling |
| R1 | ___ | Input / bias network |
| R2 | ___ | Output stability network |

## Layout Practices Used

- short audio signal paths
- dedicated audio PCB
- improved grounding
- local power decoupling
- physical separation from noisy switching paths where possible
- reduced unnecessary wire length
- controlled amplifier power

---

# 30. Volume and Audio Output Controls

The console includes a physical volume control.

```text
AUDIO INPUT

     │
     ▼

POTENTIOMETER

     │
     ▼

LM386 AMPLIFIER

     │
     ▼

SPEAKER
```

The system also supports AUX output.

A hardware switch selects the output path.

```text
                 AUDIO SOURCE

                      │
                      ▼

                 OUTPUT SWITCH

                      │
             ┌────────┴────────┐
             │                 │
             ▼                 ▼

          AUX MODE         SPEAKER MODE

             │                 │
             ▼                 ▼

       AUX OUTPUT        LM386 POWER ON

                              │
                              ▼

                         0.8 W SPEAKER
```

When the internal speaker is not required, amplifier power can be disconnected.

This reduces idle power consumption and prevents the amplifier from remaining active unnecessarily.

---

# 31. Installing RetroPie

Follow the current official RetroPie installation procedure appropriate for the operating-system image used.

After the base operating system is installed:

1. Update the system.
2. Install required packages.
3. Install RetroPie.
4. Configure the emulator.
5. Configure the controller.
6. Configure EmulationStation.
7. Validate audio.
8. Validate display performance.
9. Configure boot behavior.

Store project-specific setup notes in:

```text
docs/build-log/software-installation.md
```

Do not redistribute copyrighted game ROMs through this repository.

---

# 32. Customizing EmulationStation

The default interface was not designed for a 3.5-inch handheld display.

The theme was modified to improve readability and information density.

Changes included:

- font size
- game-title size
- metadata text size
- artwork dimensions
- spacing
- game-list readability
- interface layout
- small-screen information hierarchy

> Add default-interface screenshot.

> Add final-interface screenshot.

Store custom files in:

```text
config/emulationstation/theme/
```

The objective was to make the software interface feel designed for the physical console.

---

# 33. Game Metadata and Artwork

The game library contains more than 400 Game Boy Advance titles.

Metadata and artwork were scraped from online databases using the available scraping tools.

The frontend includes:

- game titles
- descriptions
- box art
- release information
- genres
- publisher/developer information
- additional metadata where available

```text
LOCAL GAME FILES

       │
       ▼

METADATA SCRAPER

       │
       ├── ARTWORK
       ├── DESCRIPTIONS
       ├── RELEASE INFORMATION
       ├── GENRES
       └── GAME METADATA

       │
       ▼

EMULATIONSTATION GAMELIST

       │
       ▼

VISUAL GAME LIBRARY
```

Game files and copyrighted assets should not be committed to this repository unless redistribution rights permit it.

---

# 34. Custom Splash Screens

Custom splash screens were added to make the device feel like a finished handheld console.

```text
POWER ON

    │
    ▼

BOOT PROCESS

    │
    ▼

CUSTOM SPLASH SCREEN

    │
    ▼

EMULATIONSTATION

    │
    ▼

GAME LIBRARY
```

Store the project splash screens in:

```text
assets/splashscreens/
```

> Add final splash-screen screenshot.

---

# 35. Boot Automation

The finished console automatically starts the required software services.

The startup sequence is:

```text
POWER ON

    │
    ▼

LINUX BOOT

    │
    ├── START DISPLAY DRIVER
    │
    ├── START GPIONext
    │
    ├── INITIALIZE INPUT DEVICE
    │
    ├── LOAD AUDIO CONFIGURATION
    │
    └── START RETROPIE / EMULATIONSTATION

    │
    ▼

READY TO PLAY
```

## Recommended systemd Structure

Use separate services rather than one monolithic boot script.

```text
fbcp-ili9341.service

gpionext.service

bigboy-startup.service
```

### Example Display Service Template

```ini
[Unit]
Description=BIGBOY-ADV SPI Display Driver
After=local-fs.target

[Service]
Type=simple
ExecStart=/absolute/path/to/fbcp-ili9341
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Enable the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable fbcp-ili9341.service
sudo systemctl start fbcp-ili9341.service
```

Use the actual binary location and tested service dependencies from the final system.

## Verify Services

```bash
systemctl status fbcp-ili9341.service

systemctl status <gpionext-service-name>

systemctl --failed
```

Store all final startup scripts and service files in:

```text
scripts/startup/
config/fbcp-ili9341/service/
config/gpionext/service/
```

---

# 36. System Integration

The most significant engineering work occurred during subsystem integration.

```text
MECHANICAL DESIGN
        │
        ▼
COMPONENT PLACEMENT
        │
        ▼
POWER DISTRIBUTION
        │
        ▼
THERMAL MANAGEMENT
        │
        ▼
GPIO CONFIGURATION
        │
        ▼
DISPLAY OPTIMIZATION
        │
        ▼
AUDIO NOISE REDUCTION
        │
        ▼
LINUX CONFIGURATION
        │
        ▼
BOOT AUTOMATION
        │
        ▼
USER-INTERFACE OPTIMIZATION
        │
        ▼
SYSTEM VALIDATION
```

Examples of subsystem interaction included:

- processor overclocking increased thermal and power requirements
- SPI display activity contributed to digital noise
- power-converter behavior affected Raspberry Pi stability
- power distribution affected audio quality
- GPIO allocation affected peripheral configuration
- enclosure geometry affected airflow
- component placement affected EMI and wire routing
- frontend scaling affected usability on the small display

BIGBOY-ADV therefore became a system-engineering project rather than a collection of independent modules.

---

# 37. Bring-Up Procedure

Do not assemble every subsystem and power the complete device for the first time.

A staged bring-up procedure makes failures easier to isolate.

## Stage 1 — Power System

Validate:

```text
Battery
   ↓
BMS
   ↓
Charger
   ↓
Master Switch
   ↓
Buck Converter
   ↓
5 V Output
```

Measure:

- battery voltage
- cell balance
- regulator output
- no-load current
- load voltage
- converter temperature

## Stage 2 — Raspberry Pi

Connect only the Raspberry Pi.

Verify:

```bash
vcgencmd get_throttled
```

Check for undervoltage events.

## Stage 3 — Display

Connect the TFT.

Verify:

- correct controller configuration
- stable image
- correct orientation
- SPI performance
- absence of corruption

## Stage 4 — GPIO Controls

Connect and test each button individually using:

```bash
sudo evtest
```

## Stage 5 — Audio

Test:

1. audio source
2. volume control
3. amplifier
4. speaker
5. AUX output
6. amplifier switching
7. noise with display active
8. noise during gameplay

## Stage 6 — Full Load

Run:

- display
- emulator
- speaker
- button backlights
- overclock configuration

Observe:

- voltage stability
- temperatures
- audio noise
- input reliability
- display artifacts

## Stage 7 — Final Assembly

Only after subsystem validation:

- mount electronics
- route wiring
- secure batteries
- install rear panel
- perform final continuity checks
- repeat the full-load test

---

# 38. Engineering Challenges and Solutions

| Problem | Root Cause / Limitation | Improvement |
|---|---|---|
| Boost converter failed during current spikes | Insufficient transient performance for complete system load | Changed to 2S LiPo architecture and buck regulation |
| Raspberry Pi power stability | Dynamic load requirements and distribution losses | Improved power architecture and rail distribution |
| Slow display performance | Original GoodTFT driver stack and SPI framebuffer limitations | Replaced with fbcp-ili9341 |
| Visible screen tearing | Insufficient framebuffer throughput | Tuned SPI configuration and system performance |
| Emulator/display overhead | Raspberry Pi processing requirements | Applied tested Raspberry Pi performance tuning |
| Increased thermal load | Compute, display, and converter heat | Added enclosure airflow and heatsinks |
| GPIO controls not recognized as a gamepad | Raw GPIO inputs are not standard Linux controllers | Integrated GPIONext |
| Uneven XYAB illumination | Shared LED current limiting | Used one resistor per illuminated switch |
| Audio noise | Digital switching, converter noise, wiring, grounding | Dedicated audio PCB, filtering, EMI mitigation |
| Unnecessary amplifier power consumption | Amplifier remained powered when using AUX | Added hardware power switching |
| Small-screen UI readability | Default theme designed for larger displays | Modified fonts, metadata sizing, and layout |
| Generic boot experience | Visible Linux startup process | Added custom splash screens and boot automation |
| Manual service startup | Software stack required manual initialization | Added automatic startup services/scripts |
| Complex internal packaging | Multiple electrical and mechanical subsystems | Iterative CAD and hardware-aware enclosure design |

---

# 39. Testing and Validation

A documented embedded project should include measured results.

## Power Tests

- no-load current
- boot current
- gameplay current
- peak current
- 5 V rail minimum voltage
- 5 V ripple
- buck-converter temperature
- BMS behavior
- charge current
- charging time

## Thermal Tests

- idle temperature
- gameplay temperature
- 30-minute test
- 60-minute test
- maximum recorded temperature
- throttling status

## Display Tests

- stable SPI frequency
- screen tearing
- corruption
- boot reliability
- long-duration operation

## Input Tests

- every button
- simultaneous inputs
- long presses
- repeated presses
- false events
- boot-time detection

## Audio Tests

- minimum volume
- maximum volume
- idle noise
- gameplay noise
- AUX operation
- amplifier switching
- speaker temperature

## Runtime Tests

- backlight enabled
- backlight disabled
- speaker enabled
- AUX mode
- continuous gameplay

---

# 40. Performance Results

Fill this section with final measured values.

| Metric | Result |
|---|---|
| Stable CPU Clock | ___ MHz |
| Stable Core/GPU Clock | ___ MHz |
| Stable SPI Frequency | ___ MHz |
| Average Gameplay Power | ___ W |
| Peak System Power | ___ W |
| 5 V Rail Minimum | ___ V |
| 5 V Peak-to-Peak Ripple | ___ mV |
| Idle Pi Temperature | ___ °C |
| 30-Min Gameplay Temperature | ___ °C |
| 60-Min Gameplay Temperature | ___ °C |
| Buck Converter Maximum Temperature | ___ °C |
| Battery Runtime, Backlight On | ___ h |
| Battery Runtime, Backlight Off | ___ h |
| Battery Runtime, AUX Mode | ___ h |
| Full Charge Time | ___ h |
| Boot-to-Frontend Time | ___ s |

---

# 41. Known Limitations

- SPI display bandwidth remains lower than HDMI or parallel-display interfaces.
- Overclocking increases power consumption and thermal load.
- The system currently uses off-the-shelf power modules instead of a fully integrated power-management PCB.
- Battery percentage is not currently available through a dedicated fuel-gauge IC.
- The console does not currently include a dedicated microcontroller-based safe-shutdown controller.
- Internal wiring can be further simplified through a custom motherboard or interconnect PCB.
- Final performance depends on the specific SPI display controller and panel quality.

---

# 42. Future Improvements

Potential future revisions include:

- custom main PCB
- integrated power-management PCB
- battery fuel-gauge IC
- on-screen battery percentage
- low-battery notification
- dedicated safe-shutdown controller
- custom charging and power-path management
- USB-C Power Delivery input
- higher-efficiency DC-DC regulation
- improved EMI characterization
- oscilloscope measurements of 5 V ripple
- improved audio ground-plane design
- integrated GPIO matrix
- reduced internal wiring
- modular wiring harnesses
- threaded inserts
- improved rear-panel serviceability
- higher-bandwidth display interface
- reproducible operating-system image
- automated installation script

---

# 43. Resources and Credits

## fbcp-ili9341

High-performance framebuffer copy driver for Raspberry Pi SPI displays.

https://github.com/juj/fbcp-ili9341

BIGBOY-ADV uses `fbcp-ili9341` instead of the original GoodTFT display software to improve framebuffer throughput, display responsiveness, and gaming performance.

## GPIONext

GPIO-based Linux input driver for Raspberry Pi.

https://github.com/mholgatem/GPIOnext

BIGBOY-ADV uses GPIONext to expose the physical GPIO controls as Linux input devices compatible with RetroPie and EmulationStation.

## RetroPie

Retro gaming software environment used by BIGBOY-ADV.

## EmulationStation

Graphical frontend used to browse and launch the game library.

## SolidWorks

Used to design the custom enclosure and complete mechanical assembly.

---

# 44. Project Status

**Completed Prototype — Fully Functional Embedded Linux Handheld Console**

BIGBOY-ADV successfully transformed a Raspberry Pi 3B+ development board into a self-contained embedded system integrating:

- embedded Linux
- portable power
- battery management
- USB Type-C charging
- DC-DC regulation
- dual power rails
- physical GPIO controls
- Linux input handling
- accelerated SPI graphics
- performance tuning
- thermal management
- custom audio electronics
- hardware user controls
- automated software startup
- frontend customization
- mechanical design
- 3D printing
- system-level debugging and validation

The project evolved from a Raspberry Pi gaming experiment into an exercise in complete embedded product development.

The most important engineering outcome was not simply the ability to emulate games.

It was the process of identifying system-level limitations, debugging failures, redesigning subsystems, and integrating mechanical, electrical, and software components into a reliable standalone device.

---

# Author

**Nilin M**

Electronics and Communication Engineering

Embedded Systems • Robotics • Mechatronics • Embedded Linux
