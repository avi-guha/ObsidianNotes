Overview

C1 (Limit Switch)
- Our robot must detect walls in the playing field by colliding into them and activating a limit switch
- Design a circuit that produces a voltage signal that accurately tracks the state of a limit switch 

3 Core Robotics Problems
- Sensors
- The computing/decision making
- The actuation 

Signals: Working Definitions
- Analog signal: The physical quantity varies in a range
- Digital signal: The quantity varies between 1 and 0. 

**Analog Voltage Signal**
- Must be in range \[0,3.3]V

**Digital Voltage Signal**

**ESP-32 Output Signals**
- PWM voltage signals
	- Digital Signal \[0,3.3]V
	- Continuous variable x in range 1 \[0,1] encoded as duty cycle
	- The $\frac{\text{time on}}{\text{total period}}$ is the duty cycle
- Digital Voltage Signal
	- Digital Signal \[0,3.3]V
	- Binary variable (0 or 1) encoded as voltage low or high

Fourier Theory:
- Fourier Theory: We can represent any function as a weighted sum of sine waves with different frequencies and phases
- Fourier Series: A periodic function can be represented by an infinite discrete sum of cosine waves
- Fourier Transform: An aperiodic function that can be represented by an infinite continuous sum of cosine waves. 
- Intuition: Low frequency components encode the 'big picture' variations and high frequency components encode the details of the variations

Attenuating Noise:
- we can implement high-pass // low-pass filters in order to attenuate the signals that we need. 
- Active filters? 

**Comparators**

IC Comparators:
- optimized for use as a comparator
- Often have open collector outputs

**Open-collector output** $\rightarrow$ Output of the internal amplifier drives the NPN transistor internal to the IC
- The collector and emitter are exposed for us to connect to our circuit
- We cannot drive loads directly with the collector output
- To get the digital signal corresponding to the comparator logic, you need a **pull-up resistor**

**The important thing to understand here is that we want to completely separate power and signals.** 

**Microcontrollers and GPIO pins**
- Microcontrollers use the arduino environment

```cpp title:Arduino.cpp
#include <Arduino.h>

void setup(){
#code that is run once at the beginning of the boot. 
}

void loop(){
#code that is run repeatedly
}

```
**GPIO Pins**
- Microcontrollers have general-purse input-output pins that are configurable in software
	- You have to instruct the microcontroller how to configure the pin
	- Configure pins in Arduino via pinMode() in setup()
- All GPIO pins on the Esp32 PICO are capable of pwm output
- Strapping pins have to be at a certain voltage level or left floating during boot-up or reset
	- Do not use them with an external-up or pull-down resistor that pulls to the wrong voltage level
