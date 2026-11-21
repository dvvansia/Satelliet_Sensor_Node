# Satellite Sensor Node — KiCAD Schematic

**A satellite-compatible sensor node schematic designed in KiCAD 9.0 for environmental monitoring and wireless telemetry.**

![KiCAD](https://img.shields.io/badge/KiCAD-9.0-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Schematic%20Complete-brightgreen)

---

## Overview

This project contains the complete schematic design for a satellite sensor node featuring ATmega328P microcontroller, environmental sensors (BMP280, LM35), 3.3V LDO power regulation, and XBee wireless communication module. Designed for CubeSat and nanosatellite applications requiring environmental telemetry.

**Key Features:**
- ATmega328P MCU @ 16 MHz (Arduino-compatible)
- BMP280 pressure/temperature sensor (I2C)
- LM35 temperature sensor (analog)
- XBee Zigbee wireless module
- 5V to 3.3V LDO regulation (500mA)
- ICSP + FTDI programming headers
- GPIO expansion headers
- Test points for debugging

---

## Project Structure

```
Satellite_Sensor_Node/
├── Satellite_Sensor_Node/
│   ├── Satellite_Sensor_Node.kicad_sch    # Schematic file
│   ├── Satellite_Sensor_Node.kicad_pcb    # PCB layout (placeholder)
│   ├── Satellite_Sensor_Node.kicad_pro    # Project file
│   ├── Satellite_Sensor_Node.kicad_prl    # Project local settings
│   └── Satellite_Sensor_Node-backups/     # Auto-backups
├── Results/
│   ├── Satellite_Sensor_Node.pdf          # Schematic PDF export
│   └── Satellite_Sensor_Node.svg          # Schematic SVG export
└── README.md                              # This file
```

---

## Schematic Specifications

### Block Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                      5V DC Input                             │
│                           │                                  │
│                    [Reverse Polarity]                        │
│                           │                                  │
│                    [Fuse 500mA]                              │
│                           │                                  │
│              ┌────────────┴────────────┐                     │
│              │   AMS1117-3.3 LDO       │                     │
│              │   3.3V @ 500mA max      │                     │
│              └────────────┬────────────┘                     │
│                           │                                  │
│    ┌──────────────────────┼──────────────────────┐          │
│    │                      │                      │          │
│ [ATmega328P]        [I2C Sensors]         [XBee Module]     │
│   16 MHz              BMP280                  Zigbee        │
│  Crystal            Temp/Press             Wireless TX      │
│    │                      │                      │          │
│ [LEDs]             [Analog Sensors]        [Antenna]        │
│  Status                LM35                               │
│    │                                                       │
│ [ICSP] [FTDI] [GPIO Headers] [Test Points]                 │
└─────────────────────────────────────────────────────────────┘
```

### Power Budget

| Component | Voltage | Current (avg) | Current (peak) |
|-----------|---------|---------------|----------------|
| ATmega328P | 3.3V | 5 mA | 15 mA |
| BMP280 | 3.3V | 0.3 mA | 1 mA |
| LM35 | 3.3V | 0.1 mA | 0.1 mA |
| XBee Module | 3.3V | 45 mA | 150 mA |
| LDO Quiescent | 3.3V | 5 mA | 5 mA |
| **Total** | | **55.4 mA** | **171.1 mA** |

**Calculations:**
- Input Power (peak): 5V × 171mA = **855 mW**
- LDO Power Dissipation: (5V - 3.3V) × 171mA = **291 mW**
- Regulation Efficiency: 3.3V/5V = **66%**

### Test Points

| Test Point | Net | Expected Value | Purpose |
|------------|-----|----------------|---------|
| TP_5V | 5V_IN | 4.75-5.25V | Input voltage verification |
| TP_3V3 | 3V3 | 3.14-3.47V | Regulated output |
| TP_GND | GND | 0V | Ground reference |
| TP_RST | RESET | 3.3V (high) | MCU reset status |
| TP_XTAL1 | XTAL1 | 16MHz oscillation | Clock verification |
| TP_XTAL2 | XTAL2 | 16MHz oscillation | Clock verification |
| TP_SDA | I2C_SDA | 3.3V (pulled up) | I2C data line |
| TP_SCL | I2C_SCL | 3.3V (pulled up) | I2C clock line |
| TP_TX | UART_TX | 3.3V TTL | XBee transmit |
| TP_RX | UART_RX | 3.3V TTL | XBee receive |
| TP_LM35 | ADC0 | 0-3.3V | Temperature sensor |

---

## Design Rules

### Electrical Specifications
- **Input Voltage:** 5V DC (±5%)
- **Regulated Output:** 3.3V DC (±5%)
- **Maximum Current:** 500 mA (LDO limit)
- **Operating Temperature:** -40°C to +85°C (industrial)

### Schematic Conventions
- **Trace Width:** 0.25mm minimum (signals), 0.5mm (power)
- **Clearance:** 0.2mm minimum
- **Via Size:** 0.6mm drill / 1.2mm pad
- **I2C Pull-ups:** 4.7kΩ to 3.3V
- **Crystal:** 16MHz ±20ppm, load capacitors 22pF
- **Decoupling:** 100nF ceramic per IC, 10µF bulk at LDO

### Component Placement Guidelines
- Decoupling capacitors within 5mm of IC power pins
- Crystal oscillator close to MCU pins
- Test points accessible from top layer
- Connectors aligned to board edges

---

## Usage Instructions

### 1. Open in KiCAD
```
File > Open Project > Satellite_Sensor_Node.kicad_pro
```

### 2. Review Schematic
```
Open Schematic Editor > Tools > Electrical Rules Check (ERC)
```
**Status:** ✓ No errors, no warnings

### 3. Review PCB Layout
```
Open PCB Editor > Tools > Design Rules Check (DRC)
```

### 4. Generate Manufacturing Files
```
File > Fabrication Outputs > Gerbers (.gbr)
File > Fabrication Outputs > Drill Files (.drl)
File > Fabrication Outputs > Bill of Materials (BOM)
File > Fabrication Outputs > Pick and Place (CPL)
```

### 5. Order Fabrication
Upload Gerber ZIP to:
- JLCPCB: https://jlcpcb.com
- PCBWay: https://pcbway.com

**Recommended Settings:**
- Board thickness: 1.6mm
- Copper weight: 1 oz (35µm)
- Solder mask: Green
- Silkscreen: White
- Surface finish: HASL (lead-free)

---

## BOM Summary

| Qty | Component | Value/Part | Footprint | Manufacturer |
|-----|-----------|------------|-----------|--------------|
| 1 | Microcontroller | ATmega328P-AU | TQFP-32 | Microchip |
| 1 | LDO Regulator | AMS1117-3.3 | SOT-223 | Advanced Monolithic |
| 1 | Pressure Sensor | BMP280 | LGA-8 | Bosch |
| 1 | Temperature Sensor | LM35 | TO-92 | Texas Instruments |
| 1 | Crystal | 16 MHz | HC-49/S | ECS Inc |
| 2 | Capacitor | 22pF | 0603 | Murata |
| 4 | Capacitor | 100nF | 0603 | Murata |
| 2 | Capacitor | 10µF | 0805 | Murata |
| 1 | XBee Header | XBee S2C | 2x10 2mm pitch | Digi |
| 1 | Schottky Diode | 1N5819 | DO-214 | Vishay |
| 1 | Fuse | 500mA | 1206 | Littelfuse |
| 2 | LED | Red/Green | 0603 | Kingbright |
| 3 | Resistor | 10kΩ | 0603 | Yageo |
| 2 | Resistor | 4.7kΩ | 0603 | Yageo |
| 1 | Resistor | 1kΩ | 0603 | Yageo |

*Full BOM with Digi-Key/Mouser part numbers available in `Hardware/BOM.csv`*

---

## Firmware Example

```cpp
/**
 * Satellite Sensor Node - Firmware Example
 * Reads BMP280 and LM35, transmits via XBee at 9600 baud
 */

#include <Wire.h>
#include <Adafruit_BMP280.h>

#define LM35_PIN A0
#define XBEE_BAUD 9600
#define BMP280_ADDR 0x76

Adafruit_BMP280 bmp;

void setup() {
  Serial.begin(XBEE_BAUD);
  Wire.begin();
  bmp.begin(BMP280_ADDR);
  bmp.setSampling(Adafruit_BMP280::MODE_NORMAL,
                  Adafruit_BMP280::SAMPLING_X1,
                  Adafruit_BMP280::SAMPLING_X1,
                  Adafruit_BMP280::FILTER_OFF,
                  Adafruit_BMP280::STANDBY_MS_1000);
}

void loop() {
  float temp = bmp.readTemperature();
  float pressure = bmp.readPressure() / 100.0;
  int lm35_raw = analogRead(LM35_PIN);
  float lm35_temp = (lm35_raw * 3.3 / 1024.0) * 100.0;
  
  Serial.print("T_BMP: ");
  Serial.print(temp, 2);
  Serial.print(" C | P: ");
  Serial.print(pressure, 1);
  Serial.print(" hPa | T_LM35: ");
  Serial.print(lm35_temp, 2);
  Serial.println(" C");
  
  delay(1000);
}
```

**Telemetry Output Format:**
```
T_BMP: 25.34 C | P: 1013.2 hPa | T_LM35: 24.80 C
```

---

## Applications

- **CubeSat Environmental Monitoring** — Temperature, pressure telemetry
- **Ground Station Weather Station** — Local atmospheric data
- **IoT Sensor Networks** — Wireless environmental sensing
- **Educational Satellite Projects** — Learn satellite subsystem design
- **Prototyping for Flight Hardware** — Test before space-qualified design

---

## Resume Integration

**Project Title:** Satellite Sensor Node Schematic Design (KiCAD 9.0)

**Bullet Points:**
- Designed complete schematic for satellite sensor node in KiCAD 9.0 integrating ATmega328P MCU, BMP280 pressure sensor, LM35 temperature sensor, and XBee wireless module
- Implemented 3.3V LDO power regulation with 500mA capacity, calculated power budget (171mA peak, 291mW LDO dissipation)
- Added 11 test points for debugging critical nets (power, I2C, UART, clock, ADC) enabling rapid prototyping validation
- Generated manufacturing-ready outputs (PDF, SVG, BOM) and published to GitHub for portfolio demonstration

**Skills Demonstrated:** KiCAD, Schematic Capture, Power System Design, Sensor Integration, Wireless Communication, Design for Manufacturing, Technical Documentation

**Target Roles:** Flight Hardware Engineer, Avionics Engineer, PCB Design Engineer, Embedded Systems Engineer, Satellite Systems Engineer

---

## Repository Links

| Resource | Link |
|----------|------|
| GitHub Repository | https://github.com/dvvansia/Satelliet_Sensor_Node |
| Schematic PDF | [Download](Results/Satellite_Sensor_Node.pdf) |
| Schematic SVG | [View](Results/Satellite_Sensor_Node.svg) |

---

## References

1. KiCAD 9.0 Documentation: https://www.kicad.org/doc/
2. Microchip ATmega328P Datasheet: https://ww1.microchip.com/downloads/en/DeviceDoc/ATmega328P_48P_DataSheet-40001984A.pdf
3. Bosch BMP280 Datasheet: https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bmp280-ds001.pdf
4. Texas Instruments LM35 Datasheet: https://www.ti.com/lit/ds/symlink/lm35.pdf
5. JLCPCB Capabilities: https://jlcpcb.com/capabilities

---

## License

MIT License — Educational and research use.

---

## Author

**Dhruvrajsinh Vansia**  
MSc Space Sciences and Technologies, University of Bremen  
GitHub: https://github.com/dvvansia  
Location: Bremen, Germany

---

*Part of Satellite Engineering Track portfolio projects for career in spacecraft avionics and sensor systems.*

**Other Portfolio Projects:**
- [Buck Converter LTSpice](https://github.com/dvvansia/buck_convertor_LTSpice) — 93% efficiency DC-DC converter
- [ADC Sensor Interface Proteus](https://github.com/dvvansia/adc_sensor_interface_proteus) — Multi-channel telemetry
- [RF Link Budget Calculator](https://github.com/dvvansia/RF_Link_Budget_Calculator) — MATLAB satellite link analysis
