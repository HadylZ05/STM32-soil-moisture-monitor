#  Soil Moisture Monitoring System - STM32

## Overview
This project presents a **soil moisture monitoring system** built using an **STM32 microcontroller**, a fork-type soil moisture sensor, an **LCD display**, and a **keypad-based user interface**.

The system measures soil humidity in real time, processes analog sensor data using the STM32 ADC, and displays meaningful moisture information to the user.  
Although conceptually simple, this project required **extensive hardware debugging, calibration, and embedded-level problem solving** to achieve stable and accurate results.

---

## Project Objectives
The main objectives of this project were:

- Measure soil moisture using an **analog sensor**
- Convert raw ADC values into meaningful moisture levels
- Display results clearly on an **LCD screen**
- Enable **user interaction** via a keypad
- Validate system behavior through **real soil testing**
- Gain hands-on experience with **embedded systems integration**

---

## System Description

### How the System Works
1. The soil moisture sensor is inserted into the soil  
2. The sensor outputs an analog voltage proportional to moisture  
3. The STM32 reads the signal using its ADC peripheral  
4. Raw values are mapped to moisture levels through calibration  
5. Results are displayed on the LCD  
6. The keypad allows the user to:
   - Change display modes
   - Adjust scaling
   - Reset parameters

---

## Hardware Components
The system is composed of the following hardware elements:

- **STM32 microcontroller**
- **Soil moisture sensor (fork-type probe)**
- **16×2 LCD display (I2C interface)**
- **4×3 matrix keypad**
- Power supply and connecting wires

Each component was tested individually before full integration.  
Multiple LCD units were replaced during development due to instability.

- Detailed descriptions and images are available inthe documents part.

  
---

## Author
**Hadil Zahiri**  
Computer Engineering - Embedded Systems


