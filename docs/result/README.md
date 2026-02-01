#  Results & System Validation

This section presents the **final results** of the soil moisture monitoring system and documents the behavior of the system under real operating conditions.

After extensive debugging, calibration, and hardware testing, the system reached a **stable and reliable state**, successfully measuring soil moisture levels and displaying meaningful data in real time.

---

##  Final System Behavior

Once fully assembled and calibrated, the system demonstrated the following capabilities:

- Accurate detection of soil moisture variations
- Real-time display of values on the LCD
- Stable keypad interaction without glitches
- Consistent behavior across multiple test scenarios

The LCD correctly displayed:
- Raw ADC values
- Moisture percentage values
- User-selected display modes

No visual corruption, random characters, or display instability were observed after final calibration.

---

##  Moisture Detection Results

The soil moisture sensor was tested under different conditions to validate its responsiveness and accuracy:

### Test Scenarios
- **Dry soil**
- **Moist soil**
- **Sensor inserted into very wet soil**
- **Sensor wrapped in a wet cloth (extreme case)**

### Observed Behavior
- ADC values **decreased as soil moisture increased**
- Moisture percentage values updated **in real time**
- Transitions between dry and wet conditions were smooth and consistent

This confirmed that:
- The ADC configuration was correct
- The calibration constants were properly chosen
- The mapping function produced meaningful real-world results

---

##  Display Output Verification

The LCD display was monitored throughout testing to ensure correctness and stability.

### Display Observations
- Numerical values were clearly readable
- No random symbols or corrupted characters appeared
- Display updated smoothly without flickering
- Mode switching via keypad worked as expected

> Earlier development stages suffered from unstable LCD output.  
> After careful wiring checks, timing adjustments, and repeated testing, the display became fully reliable.

---


## Experimental Results - Visual Evidence

The following images document the system during real operation and serve as visual proof of correct functionality, calibration, and stability.

---

### LCD Output – Real-Time Readings

This image shows a close-up of the LCD displaying live system data.

- Moisture percentage is shown in real time
- Raw ADC value is displayed simultaneously
- Display mode and scaling factor are clearly indicated
- No corrupted characters or unstable behavior observed

![LCD Close-Up Output](./result_images/lcd_closeup.jpg)

---

### Full System Integration Test

This image demonstrates the complete system operating together:

- STM32 microcontroller running the application
- Matrix keypad connected and actively used for user input
- LCD updating dynamically based on sensor readings
- Stable wiring and continuous operation

This confirms successful integration of hardware and software components.

![Full System with Keypad and LCD](./result_images/full_system.jpg)

---


##  Result Summary

Based on visual observation and numerical output:

- Soil moisture readings respond correctly to environmental changes
- ADC values decrease as moisture increases
- Mapped percentage values remain consistent and intuitive
- LCD output is stable and readable
- Keypad interaction functions reliably

The system successfully meets its design objectives and operates as intended under real conditions.

