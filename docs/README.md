#Documentation
this section focuses on the physical components used in the system and their role in measuring and displaying soil moisture.

###Components Overview

#### 1. STM32 Microcontroller
![STM32](component_images/STM32.png)

The STM32 microcontroller is the core of the system.  
It is responsible for:
- Reading the analog signal from the soil moisture sensor using the **ADC**
- Processing raw sensor values
- Mapping the readings to meaningful moisture levels or percentages
- Driving the LCD display
- Handling keypad input for user interaction

All system logic runs on the STM32, making it the central control unit of the project.

---

#### 2. Soil Moisture Sensor (Fork-Type)
![Moisture Sensor](hardware/component_images/Moisture-sensor.png)

The soil moisture sensor uses two probes inserted into the soil to measure electrical conductivity, which varies depending on water content.

In this project:
- The sensor outputs an **analog voltage**
- The voltage increases or decreases based on soil moisture
- This signal is read by the STM32 ADC
- Extensive **calibration** was required to map raw values to accurate moisture readings

This sensor was tested in both dry and wet soil conditions to validate accuracy.

---

#### 3. Keypad
![Keypad](hardware/component_images/keypad.png)

The keypad provides a simple user interface for interacting with the system.

It is used to:
- Switch between display modes
- Control how moisture data is presented
- Enable user-driven interaction without reprogramming the board

The keypad is scanned by the STM32 using digital GPIO inputs.

---

#### 4. Breadboard and Connecting Wires
![Breadboard and Wires](hardware/component_images/BreadBoard-wires.png)

The breadboard and jumper wires were used to:
- Assemble the circuit without soldering
- Connect the STM32, sensor, keypad, and display
- Allow easy debugging and hardware changes during development

This setup was especially important during testing and calibration phases.


