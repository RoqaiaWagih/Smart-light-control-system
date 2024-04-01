# Smart-light-control-system
**Overview:**
This repository contains code for controlling room lighting based on the light intensity sensed by an LDR (Light Dependent Resistor). The system allows setting a threshold value for the light intensity using a keypad. The light intensity is measured using an ADC (Analog to Digital Converter) channel and displayed on an LCD (Liquid Crystal Display). If the light intensity exceeds the set threshold, the system adapts the room lighting accordingly using an LED.

**Hardware Components:**
1. AVR-Atmega32 microcontroller
2. LDR (Light Dependent Resistor)
3. Keypad
4. LCD Display
5. LED
6. ADC (Analog to Digital Converter)

**Architecture Layers:**
1. **MCAL (Microcontroller Abstraction Layer):** This layer provides a hardware abstraction interface for the microcontroller peripherals, such as ADC and GPIO, allowing portability across different microcontroller architectures.

2. **HAL (Hardware Abstraction Layer):** HAL builds upon the MCAL and provides higher-level, standardized interfaces for hardware components like LCD, keypad, and LED, abstracting away low-level hardware details.

3. **Application Layer:** This layer contains the main application logic, including reading the light intensity from the LDR using ADC, processing the data to determine the light intensity percentage, and controlling the LED based on the threshold value.

4. **Utilities:** Utilities layer may include helper functions, macros, or data structures used across the application to improve code modularity and reusability.



**Usage:**
1. **Setup:** Connect the hardware components as per the circuit diagram provided.
   
2. **Upload Code:** Upload the code provided in the repository to the microcontroller.

3. **Set Threshold:** Use the keypad to set the threshold value for the light intensity.

4. **Observe Light Intensity:** The LCD will display the current light intensity percentage.

5. **Adapt Room Lighting:** If the light intensity exceeds the threshold, the LED will adjust room lighting accordingly.

**Note:** Make sure to refer to the datasheets of the components used and adjust the code according to the specific hardware setup. Additionally, provide proper documentation and comments within the code for clarity and ease of understanding.
