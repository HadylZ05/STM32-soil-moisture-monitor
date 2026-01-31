#  Documentation  

##  Overview

This document describes the **software architecture and implementation** of the STM32-based Soil Moisture Monitoring System.

The software is responsible for:
- Acquiring analog data from a soil moisture sensor
- Converting raw ADC values into meaningful moisture percentages
- Displaying results on an LCD
- Allowing real-time user interaction through a keypad

The project was developed using **STM32CubeIDE**, written in **C**, and built on top of **STM32 HAL (Hardware Abstraction Layer) libraries** to ensure portability, clarity, and reliability.

---

##  Development Environment

- **IDE:** STM32CubeIDE  
- **Programming Language:** C  
- **Libraries:** STM32 HAL  
- **Target MCU:** STM32 (ADC, GPIO, I2C peripherals)

HAL libraries were used for peripheral initialization and low-level hardware access, while application logic was implemented inside user code sections.

---

##  Software Architecture

The software is organized around the following modules:

- **ADC Module** – Reads analog voltage from the soil moisture sensor  
- **Calibration & Mapping Logic** – Converts raw ADC values to percentages  
- **LCD Driver (I2C)** – Displays system output  
- **Keypad Interface** – Allows user control and interaction  
- **Main Control Loop** – Coordinates all system operations  

---

##  STM32CubeMX / `.ioc` Configuration

The project configuration was created using **STM32CubeMX** via the `.ioc` file.  
This file defines all peripheral settings and pin assignments before code generation.

![STM32CubeMX Configuration](./ioc_configuration.png)

## Configuration Highlights

### ADC (Analog-to-Digital Converter)
- Enabled on a **single channel** connected to the soil moisture sensor  
- **12-bit resolution** for precise measurements  
- **Software-triggered conversion** for controlled sampling  

### I2C
- Enabled for **LCD communication**  
- Reduces GPIO usage and simplifies wiring  
- Improves modularity and scalability  

### GPIO
- Configured for **keypad rows and columns**  
- Used for **sensor input** and **user interaction**  

### `.ioc` Configuration Benefits
The STM32CubeMX `.ioc` file ensures:
- Clean and consistent peripheral initialization  
- Reliable hardware–software mapping  
- Easier debugging and future modifications  

---

## 🔁 Main Program Flow

```c
int main(void)
{
    HAL_Init();
    SystemClock_Config();

    MX_GPIO_Init();
    MX_ADC_Init();
    MX_I2C1_Init();

    LCD_Init(&hi2c1);
    HAL_Delay(50);
    LCD_SetCursor(0, 0);
    LCD_SendString("Hello, world!");

    while (1)
    {
        // Read sensor, process data, handle keypad, update LCD
    }
}

```
##  Execution Steps

The software follows a clear and deterministic execution flow to ensure reliable real-time operation:

1. HAL initialization and system clock configuration  
2. GPIO, ADC, and I2C peripheral initialization  
3. LCD initialization and display of a test message  
4. Entry into an infinite loop for continuous real-time operation  

---

##  ADC Configuration and Reading

The Analog-to-Digital Converter (ADC) is configured to read the analog voltage produced by the soil moisture sensor.

### ADC Configuration Details
- 12-bit resolution  
- Single conversion mode  
- Software-triggered conversion  

### ADC Initialization Code
```c
static void MX_ADC_Init(void)
{
    hadc.Instance = ADC1;
    hadc.Init.Resolution = ADC_RESOLUTION_12B;
    hadc.Init.ContinuousConvMode = DISABLE;
    hadc.Init.ExternalTrigConv = ADC_SOFTWARE_START;

    HAL_ADC_Init(&hadc);

    ADC_ChannelConfTypeDef sConfig = {0};
    sConfig.Channel = ADC_CHANNEL_0;
    sConfig.Rank = ADC_RANK_CHANNEL_NUMBER;
    sConfig.SamplingTime = ADC_SAMPLETIME_1CYCLE_5;

    HAL_ADC_ConfigChannel(&hadc, &sConfig);
}
```
### Purpose

The ADC is used to read the analog voltage produced by the soil moisture sensor.

- Reads analog voltage from the soil moisture sensor  
- Produces raw ADC values approximately ranging from:
  - **~300** → very wet soil  
  - **~3500** → very dry soil  

---

###  Moisture Calibration & Mapping

Raw ADC values are converted into a percentage scale to make the data more intuitive and user-friendly.

#### Calibration Constants
```c
const uint32_t wet_raw = 300;
const uint32_t dry_raw = 3500;
```






