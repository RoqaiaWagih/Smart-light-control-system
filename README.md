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
**code**

```c

#include <avr/io.h>
/*********************MCAL******************************/
#include "DIO_interface.h"
#include "ADC_interface.h"
/*********************HAL*******************************/
#include "LCD.h"
#include "KPD.h"

/*********************UTILTES_TYPES*********************/
#include "STD_TYPES.h"
#include "BIT_MATH.h"

// Define constants
#define F_CPU 16000000UL
#define THRESHOLD_MAX 1023
// Include delay library
#include <util/delay.h>


void initialize();
void Light_Control(u16 AnalogValue, u16 Threshold);
u16 Key_Press(void);
int main(void)
{
	//CODE sequence:
	//init(): Initializes necessary peripherals.
	//while(1): Infinite loop for continuous operation.
	//Reads keypad input to set the threshold value.
	//Reads the LDR value using ADC.
	//Displays the percentage of light on the LCD.
	//Adapts room light based on the ADC value.
	//Adds a delay to prevent flickering.
	
	// Initialize hardware peripheral components
	initialize();
	
	// Define variables
	u16 Digital;
	u16 AnalogValue;
	u16 percentage;
	u16 Threshold;
    /* Replace with your application code */
    while (1) 
    {
		u8 myString1[] = "Enter threshold:";
		LCD_voidWriteString(myString1);
		 // Read threshold value from keypad
		Threshold =Key_Press();
		 // Read light value from LDR using ADC
		ADC_voidGetDigitalValue(ADC_CHANNEL_0,&Digital);
		
		// Calculate percentage of light
		percentage = Digital / THRESHOLD_MAX * 100;
		// Display percentage on LCD
		LCD_voidWriteString("Light: ");
		LCD_voidWriteString(percentage);
		LCD_voidWriteString("%");
		// If light is above threshold, turn on LED
		Light_Control( percentage, Threshold);
		_delay_ms(2000);
    }
	
}

void initialize() {
	// Initialize components
	KPD_INIT();
	LCD_voidInit();
	ADC_voidInit(ADC_REFERENCE_INTRNAL);
	
	// Initialize LDR pin direction
	DIO_voidSetPinDirection (DIO_PORTA,DIO_PIN2,DIO_PIN_INPUT);
	// Initialize LED pin direction
	DIO_voidSetPinDirection (DIO_PORTA,DIO_PIN7,DIO_PIN_OUTPUT);
	
}
u16 Key_Press(void)
{
	u16 threshold; 
	u8 Local_u8PressedKey =  KPD_u8GetPressedKey();
	if (Local_u8PressedKey  != '\0' && Local_u8PressedKey >= '0' && Local_u8PressedKey <= '9') {
		threshold = threshold * 10 + (Local_u8PressedKey - '0');
	}
	return threshold;
}

//Light Control
void Light_Control(u16 AnalogValue, u16 Threshold)
{
	if(AnalogValue > Threshold)
	{
		DIO_voidSetPinValue(DIO_PORTA, DIO_PIN7, DIO_PIN_LOW);
	}
	else
	{
		DIO_voidSetPinValue(DIO_PORTA, DIO_PIN7, DIO_PIN_HIGH);
	}
}
```


**Usage:**
1. **Setup:** Connect the hardware components as per the circuit diagram provided.
   
2. **Upload Code:** Upload the code provided in the repository to the microcontroller.

3. **Set Threshold:** Use the keypad to set the threshold value for the light intensity.

4. **Observe Light Intensity:** The LCD will display the current light intensity percentage.

5. **Adapt Room Lighting:** If the light intensity exceeds the threshold, the LED will adjust room lighting accordingly.

**Note:** Make sure to refer to the datasheets of the components used and adjust the code according to the specific hardware setup. Additionally, provide proper documentation and comments within the code for clarity and ease of understanding.
