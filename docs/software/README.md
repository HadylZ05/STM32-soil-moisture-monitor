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

![STM32CubeMX Configuration](./ioc-configuration.png)

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

##  Main Program Flow

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
### Mapping Function

The raw ADC value is converted into a percentage representing soil moisture.

```c
uint32_t map_moisture(uint32_t raw)
{
    if (raw <= wet_raw) return 100;
    if (raw >= dry_raw) return 0;

    return 100 - ((raw - wet_raw) * 100) / (dry_raw - wet_raw);
}
```
**Output Range**
-0% → Dry soil
-100% → Wet soil


##  LCD Handling (I2C)

The LCD is controlled using the **I2C protocol** to minimize wiring complexity and reduce GPIO usage.

### LCD Interface Functions
```c
void lcd_send_cmd(char cmd);
void lcd_send_data(char data);
void lcd_send_string(char *str);
void lcd_init(void);
```
### Advantages

Cleaner hardware design

Reliable communication

Modular and reusable display driver

##  Keypad Input Handling

A matrix keypad enables interactive control of the system, allowing the user to adjust display modes and scaling factors in real time.

### Keypad Interface Function
char read_keypad(void);

### Supported Controls
- **\*** key → Enter scaling mode  
- **0–9 keys** → Set scaling factor  
- **# key** → Reset scale  

Keypad scanning runs continuously inside the main loop to ensure real-time responsiveness.

---

##  Display Modes

The system supports **three display modes**:

- **Mode 0** – Raw ADC value **and** moisture percentage  
- **Mode 1** – Raw ADC value only  
- **Mode 2** – Moisture percentage only  
##  Display Mode Control

### Display Mode Variable

```c
static uint8_t display_mode = 0;
```
##  Main Control Loop Logic

Inside the infinite loop, the system performs the following operations continuously:

- Reads the **ADC value** from the soil moisture sensor  
- Maps the raw value to a **moisture percentage**  
- Reads **keypad input** for user interaction  
- Updates the **display mode** or **scaling factor** based on user input  
- Refreshes the **LCD output** in real time  

This logic enables **continuous monitoring**, **interactive control**, and **stable real-time operation** of the system.

## Testing & Validation

The system was tested under multiple real-world conditions to verify accuracy and stability:

### Test Conditions
- Dry soil  
- Moist soil  
- Sensor wrapped in a wet towel  

### Results
- ADC values **decreased as soil moisture increased**
- Moisture **percentage output updated in real time**
- LCD display and keypad **remained fully responsive**
- No runtime errors or system instability were observed during testing

##  Error Handling

The system includes a basic but effective error handling mechanism to ensure safe operation in case of critical failures.

```c
void Error_Handler(void)
{
    __disable_irq();
    while (1)
    {
        // System halted on error
    }
}
```



